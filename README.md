using System;
using System.Collections.Concurrent;
using System.Configuration;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Xml.Linq;
using IdentityModel.OidcClient;

namespace OidcGaiaConnector
{
    public class OidcClientConfig
    {
        public string Authority { get; private set; }
        public string ClientId { get; private set; }
        public string RedirectUrl { get; private set; }
        public string Scope { get; private set; }

        public OidcClientConfig(string authority, string clientId, string redirectUrl, string scope = "openid profile")
        {
            Authority = authority;
            ClientId = clientId;
            RedirectUrl = redirectUrl;
            Scope = scope;
        }
    }

    public interface IIdpClient
    {
        Task<TokenResponse> GetAccessTokenFromIDP();
        Task<TokenResponse> RefreshAccessTokenAsync(string refreshToken);
    }

    public interface IAuthorizationHeaderSetter
    {
        void SetAuthorizationHeader(HttpClient httpClient, string accessToken);
    }

    public interface IHttpListener : IDisposable
    {
        void Start();
        Task<HttpListenerContext> GetContextAsync();
        void Stop();
    }

    public interface IProcessStarter
    {
        void Start(string url);
    }

    public interface IOidcClient
    {
        Task<LoginResult> LoginAsync(LoginRequest request);
    }

    public interface IEnvironmentTypeResolver
    {
        string ResolveEnvironmentType(string environment);
    }

    public interface IGaiaUrlProvider
    {
        string GetUrl(string environmentType);
    }

    public interface IGaiaEndpointProvider
    {
        string GetGaiaEndpoint(string environment);
    }

    public interface ITokenStorage
    {
        Task SaveTokensAsync(string accessToken, string refreshToken, DateTime accessTokenExpiryTime);
        Task<TokenInfo> GetAccessTokenAsync();
        Task<string> GetRefreshTokenAsync();
        Task ClearTokensAsync();
    }

    public interface ITokenManager
    {
        Task<string> GetValidAccessTokenAsync();
        Task<bool> IsTokenValidAsync();
        Task ClearTokensAsync();
    }

    public class TokenInfo
    {
        public string AccessToken { get; set; }
        public DateTime ExpiryTime { get; set; }
    }

    public class TokenResponse
    {
        public string AccessToken { get; set; }
        public string RefreshToken { get; set; }
        public int ExpiresIn { get; set; }
    }

    public class HttpListenerWrapper : IHttpListener
    {
        private readonly HttpListener _listener = new HttpListener();

        public HttpListenerWrapper(string prefix)
        {
            _listener.Prefixes.Add(prefix);
        }

        public void Start() => _listener.Start();
        public Task<HttpListenerContext> GetContextAsync() => _listener.GetContextAsync();
        public void Stop() => _listener.Stop();
        public void Dispose() => _listener.Close();
    }

    public class ProcessStarter : IProcessStarter
    {
        public void Start(string url)
        {
            Process.Start(new ProcessStartInfo
            {
                FileName = url,
                UseShellExecute = true,
            });
        }
    }

    public class OidcClientWrapper : IOidcClient
    {
        private readonly OidcClient _client;

        public OidcClientWrapper(OidcClientOptions options)
        {
            _client = new OidcClient(options);
        }

        public Task<LoginResult> LoginAsync(LoginRequest request)
        {
            return _client.LoginAsync(request);
        }
    }

    public class OidcIdpClient : IIdpClient
    {
        private readonly OidcClientConfig _config;
        private readonly IHttpListener _httpListener;
        private readonly IProcessStarter _processStarter;
        private readonly IOidcClient _oidcClient;

        public OidcIdpClient(
            OidcClientConfig config,
            IHttpListener httpListener,
            IProcessStarter processStarter,
            IOidcClient oidcClient)
        {
            _config = config;
            _httpListener = httpListener;
            _processStarter = processStarter;
            _oidcClient = oidcClient;
        }

        public async Task<TokenResponse> GetAccessTokenFromIDP()
        {
            Console.WriteLine("Redirect URI: " + _config.RedirectUrl);

            _httpListener.Start();
            Console.WriteLine("Listening...");

            var result = await _oidcClient.LoginAsync(new LoginRequest());

            Console.WriteLine("Start URL: " + result.State.StartUrl);

            _processStarter.Start(result.State.StartUrl);

            var context = await _httpListener.GetContextAsync();
            var response = context.Response;
            var responseString = "<html><head><meta http-equiv='refresh' content='10;url=" + _config.Authority + "'></head><body>Please return to the app.</body></html>";
            var buffer = Encoding.UTF8.GetBytes(responseString);
            response.ContentLength64 = buffer.Length;
            var responseOutput = response.OutputStream;
            await responseOutput.WriteAsync(buffer, 0, buffer.Length);
            responseOutput.Close();

            _httpListener.Stop();

            if (result.IsError)
            {
                throw new Exception("Error getting access token: " + result.Error);
            }

            return new TokenResponse
            {
                AccessToken = result.AccessToken,
                RefreshToken = result.RefreshToken,
                ExpiresIn = result.AccessTokenExpiration.HasValue 
                    ? (int)(result.AccessTokenExpiration.Value - DateTime.UtcNow).TotalSeconds 
                    : 3600 // Default to 1 hour if expiration is not provided
            };
        }

        public async Task<TokenResponse> RefreshAccessTokenAsync(string refreshToken)
        {
            // This is a placeholder. You need to implement the actual refresh token logic
            // using your OIDC client library or a direct HTTP call to your token endpoint.
            throw new NotImplementedException("Refresh token functionality needs to be implemented.");
        }
    }

    public class AuthorizationHeaderSetter : IAuthorizationHeaderSetter
    {
        public void SetAuthorizationHeader(HttpClient httpClient, string accessToken)
        {
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        }
    }

    public class AppConfigEnvironmentResolver : IEnvironmentTypeResolver
    {
        private readonly ConcurrentDictionary<string, string> environmentTypeCache;

        public AppConfigEnvironmentResolver(string configPath)
        {
            var configDoc = XDocument.Load(configPath);
            var environmentList = ParseEnvironmentListFromConfig(configDoc);

            environmentTypeCache = new ConcurrentDictionary<string, string>(
                environmentList.ToDictionary(
                    env => env,
                    env => CategorizeEnvironmentType(env)
                ),
                StringComparer.OrdinalIgnoreCase
            );
        }

        private IEnumerable<string> ParseEnvironmentListFromConfig(XDocument configDoc)
        {
            return configDoc.Root.Element("appSettings")
                .Elements("add")
                .First(e => e.Attribute("key").Value == "Environments")
                .Attribute("value").Value
                .Split(',');
        }

        private string CategorizeEnvironmentType(string environment)
        {
            if (environment.Contains("PROD")) return "PROD";
            if (environment.Contains("UAT") || environment.Contains("PTE")) return "UAT";
            if (environment.Contains("SIT")) return "SIT";
            if (environment.Contains("DEV")) return "DEV";
            if (environment.Contains("PREPROD")) return "PREPROD";
            return "SIT"; // Default to SIT
        }

        public string ResolveEnvironmentType(string environment)
        {
            string resolvedType;
            return environmentTypeCache.TryGetValue(environment, out resolvedType) 
                ? resolvedType 
                : CategorizeEnvironmentType(environment);
        }
    }

    public class AppSettingsGaiaUrlProvider : IGaiaUrlProvider
    {
        private readonly ConcurrentDictionary<string, string> gaiaUrlCache;

        public AppSettingsGaiaUrlProvider()
        {
            gaiaUrlCache = new ConcurrentDictionary<string, string>(
                new[]
                {
                    new KeyValuePair<string, string>("SIT", ConfigurationManager.AppSettings["SitGaiaUrl"]),
                    new KeyValuePair<string, string>("UAT", ConfigurationManager.AppSettings["UatGaiaUrl"]),
                    new KeyValuePair<string, string>("PROD", ConfigurationManager.AppSettings["ProdGaiaUrl"]),
                    new KeyValuePair<string, string>("DEV", ConfigurationManager.AppSettings["DevGaiaUrl"]),
                    new KeyValuePair<string, string>("PREPROD", ConfigurationManager.AppSettings["PreprodGaiaUrl"])
                },
                StringComparer.OrdinalIgnoreCase
            );
        }

        public string GetUrl(string environmentType)
        {
            string url;
            if (gaiaUrlCache.TryGetValue(environmentType, out url))
            {
                return url;
            }
            throw new ArgumentException("No URL configured for environment type: " + environmentType);
        }
    }

    public class GaiaEndpointProvider : IGaiaEndpointProvider
    {
        private readonly IEnvironmentTypeResolver environmentResolver;
        private readonly IGaiaUrlProvider urlProvider;

        public GaiaEndpointProvider(
            IEnvironmentTypeResolver environmentResolver,
            IGaiaUrlProvider urlProvider)
        {
            this.environmentResolver = environmentResolver;
            this.urlProvider = urlProvider;
        }

        public string GetGaiaEndpoint(string environment)
        {
            var environmentType = environmentResolver.ResolveEnvironmentType(environment);
            return urlProvider.GetUrl(environmentType);
        }
    }

    public class TokenStorage : ITokenStorage
    {
        private string _accessToken;
        private string _refreshToken;
        private DateTime _accessTokenExpiryTime;

        public Task SaveTokensAsync(string accessToken, string refreshToken, DateTime accessTokenExpiryTime)
        {
            _accessToken = accessToken;
            _refreshToken = refreshToken;
            _accessTokenExpiryTime = accessTokenExpiryTime;
            return Task.FromResult(0); // Task.CompletedTask equivalent in .NET 4.6.1
        }

        public Task<TokenInfo> GetAccessTokenAsync()
        {
            return Task.FromResult(new TokenInfo
            {
                AccessToken = _accessToken,
                ExpiryTime = _accessTokenExpiryTime
            });
        }

        public Task<string> GetRefreshTokenAsync()
        {
            return Task.FromResult(_refreshToken);
        }

        public Task ClearTokensAsync()
        {
            _accessToken = null;
            _refreshToken = null;
            _accessTokenExpiryTime = DateTime.MinValue;
            return Task.FromResult(0);
        }
    }

    public class TokenManager : ITokenManager
    {
        private readonly IIdpClient _idpClient;
        private readonly ITokenStorage _tokenStorage;

        public TokenManager(IIdpClient idpClient, ITokenStorage tokenStorage)
        {
            _idpClient = idpClient;
            _tokenStorage = tokenStorage;
        }

        public async Task<string> GetValidAccessTokenAsync()
        {
            var tokenInfo = await _tokenStorage.GetAccessTokenAsync();

            if (string.IsNullOrEmpty(tokenInfo.AccessToken) || DateTime.UtcNow >= tokenInfo.ExpiryTime)
            {
                // Access token is missing or expired, try to refresh
                string refreshToken = await _tokenStorage.GetRefreshTokenAsync();
                if (!string.IsNullOrEmpty(refreshToken))
                {
                    // Attempt to refresh the token
                    try
                    {
                        TokenResponse refreshResponse = await _idpClient.RefreshAccessTokenAsync(refreshToken);
                        await _tokenStorage.SaveTokensAsync(
                            refreshResponse.AccessToken,
                            refreshResponse.RefreshToken,
                            DateTime.UtcNow.AddSeconds(refreshResponse.ExpiresIn));
                        return refreshResponse.AccessToken;
                    }
                    catch
                    {
                        // Refresh token might be invalid or expired
                        await _tokenStorage.ClearTokensAsync();
                    }
                }

                // If we couldn't refresh, get a new token
                TokenResponse newTokenResponse = await _idpClient.GetAccessTokenFromIDP();
                await _tokenStorage.SaveTokensAsync(
                    newTokenResponse.AccessToken,
                    newTokenResponse.RefreshToken,
                    DateTime.UtcNow.AddSeconds(newTokenResponse.ExpiresIn));
                return newTokenResponse.AccessToken;
            }

            return tokenInfo.AccessToken;
        }

        public async Task<bool> IsTokenValidAsync()
        {
            var tokenInfo = await _tokenStorage.GetAccessTokenAsync();
            return DateTime.UtcNow < tokenInfo.ExpiryTime;
        }

        public Task ClearTokensAsync()
        {
            return _tokenStorage.ClearTokensAsync();
        }
    }

    public static class IdpClientFactory
    {
        public static IIdpClient CreateOidcClient(OidcClientConfig config)
        {
            var httpListener = new HttpListenerWrapper(config.RedirectUrl);
            var processStarter = new ProcessStarter();
            var oidcClient = new OidcClientWrapper(new OidcClientOptions
            {
                Authority = config.Authority,
                ClientId = config.ClientId,
                Scope = config.Scope,
                RedirectUri = config.RedirectUrl,
            });

            return new OidcIdpClient(config, httpListener, processStarter, oidcClient);
        }
    }

    public static class AuthorizationHeaderSetterFactory
    {
        public static IAuthorizationHeaderSetter CreateAuthorizationHeaderSetter()
        {
            return new AuthorizationHeaderSetter();
        }
    }

    public static class GaiaEndpointProviderFactory
    {
        private static readonly Lazy<IGaiaEndpointProvider> lazyProviderInstance = 
            new Lazy<IGaiaEndpointProvider>(() => CreateProviderInstance(), System.Threading.LazyThreadSafetyMode.ExecutionAndPublication);

        private static IGaiaEndpointProvider CreateProviderInstance()
        {
            string configPath = Path.Combine(Application.StartupPath, "App.config");
            var environmentResolver = new AppConfigEnvironmentResolver(configPath);
            var urlProvider = new AppSettingsGaiaUrlProvider();
            return new GaiaEndpointProvider(environmentResolver, urlProvider);
        }

        public static IGaiaEndpointProvider Create()
        {
            return lazyProviderInstance.Value;
        }
    }

    public class GaiaClient
    {
        private readonly ITokenManager _tokenManager;
        private readonly IAuthorizationHeaderSetter _headerSetter;
        private readonly IGaiaEndpointProvider _endpointProvider;
        private readonly HttpClient _http

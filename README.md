// OidcGaiaConnector.cs
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
        public string Authority { get; }
        public string ClientId { get; }
        public string RedirectUrl { get; }
        public string Scope { get; }

        public OidcClientConfig(string authority, string clientId, string redirectUrl, string scope = "openid profile")
        {
            Authority = authority;
            ClientId = clientId;
            RedirectUrl = redirectUrl;
            Scope = scope;
        }
    }

    public class GaiaClient
    {
        private readonly IIdpClient _idpClient;
        private readonly IAuthorizationHeaderSetter _headerSetter;
        private readonly IGaiaEndpointProvider _endpointProvider;
        private readonly HttpClient _httpClient;

        public GaiaClient(IIdpClient idpClient, IAuthorizationHeaderSetter headerSetter, IGaiaEndpointProvider endpointProvider)
        {
            _idpClient = idpClient;
            _headerSetter = headerSetter;
            _endpointProvider = endpointProvider;
            _httpClient = new HttpClient();
        }

        public async Task<string> GetGaiaData(string environment)
        {
            var token = await _idpClient.GetAccessTokenFromIDP();
            _headerSetter.SetAuthorizationHeader(_httpClient, token);

            var endpoint = _endpointProvider.GetGaiaEndpoint(environment);
            var response = await _httpClient.GetAsync(endpoint);

            response.EnsureSuccessStatusCode();
            return await response.Content.ReadAsStringAsync();
        }
    }

    public interface IIdpClient
    {
        Task<string> GetAccessTokenFromIDP();
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

        public async Task<string> GetAccessTokenFromIDP()
        {
            Console.WriteLine($"Redirect URI: {_config.RedirectUrl}");

            _httpListener.Start();
            Console.WriteLine("Listening...");

            var result = await _oidcClient.LoginAsync(new LoginRequest());

            Console.WriteLine($"Start URL: {result.State.StartUrl}");

            _processStarter.Start(result.State.StartUrl);

            var context = await _httpListener.GetContextAsync();
            var response = context.Response;
            var responseString = $"<html><head><meta http-equiv='refresh' content='10;url={_config.Authority}'></head><body>Please return to the app.</body></html>";
            var buffer = Encoding.UTF8.GetBytes(responseString);
            response.ContentLength64 = buffer.Length;
            var responseOutput = response.OutputStream;
            await responseOutput.WriteAsync(buffer, 0, buffer.Length);
            responseOutput.Close();

            _httpListener.Stop();

            if (result.IsError)
            {
                throw new Exception($"Error getting access token: {result.Error}");
            }

            return result.AccessToken;
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
            return environmentTypeCache.GetOrAdd(environment, CategorizeEnvironmentType);
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
            if (gaiaUrlCache.TryGetValue(environmentType, out var url))
            {
                return url;
            }
            throw new ArgumentException($"No URL configured for environment type: {environmentType}");
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
}

class Program
{
    static async Task Main(string[] args)
    {
        var oidcConfig = new OidcGaiaConnector.OidcClientConfig(
            authority: "https://your-authority-url",
            clientId: "your-client-id",
            redirectUrl: "http://localhost:port/"
        );

        var idpClient = OidcGaiaConnector.IdpClientFactory.CreateOidcClient(oidcConfig);
        var headerSetter = OidcGaiaConnector.AuthorizationHeaderSetterFactory.CreateAuthorizationHeaderSetter();
        var gaiaEndpointProvider = OidcGaiaConnector.GaiaEndpointProviderFactory.Create();

        var gaiaClient = new OidcGaiaConnector.GaiaClient(idpClient, headerSetter, gaiaEndpointProvider);

        try
        {
            string environment = "YOUR_ENVIRONMENT";
            string gaiaData = await gaiaClient.GetGaiaData(environment);
            Console.WriteLine($"Received Gaia data: {gaiaData}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An error occurred: {ex.Message}");
        }
    }
}

// App.config
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <add key="Environments" value="CHFRTBSA,CVA,ERCFT01,ERCFT01SA,ERCFT02,ERCFT03,ERCFT04,ERCFT05,ERCFT05-ACVA,ERCFT05-http,FRTBFT01,FRTBFT01SA,FRTBFT02,FRTBFT02SA,FRTBFT03,FRTBFT03SA,FRTBFT04,FRTBFT05,FRTBFT06,FRTBFT06SA,FRTBSA,FT11,FT11_SECURE,FT12,FT13,FT14,FT15,FT16,FT17,FT18,FT19,FT19-MYRIAD,FT20,FT21,FT21-ACVA,FT22,FT22-ACVA,FT23,FT23-ACVA,FT24,FT24SA,FT25,FT26,FT27,FT28,FT29,FT30,FT31,FT32,FT32SA,FT33,FT34,FT34-ERC,FT35,FT35-ACVA,FT36,FT37,FT38,FT39,FT40,FT41,LT11,LT12,LT12-MYRIAD,LT13,LT14,LT15,LT16,LT17,LT18,LT19,LT20,LT21,LT23,LT24,LT25,LT26,LT27,LT27-JIDL,LT28,PRD0,PIE,PTE-FRTB,PTE2,PTE2-FRTBSA,UAT1,UAT1-MYRIAD,UAT1_One,UAT10,UAT11,UAT12,UAT13,UAT14,UAT1REFTIDL,UAT2,UAT2REFTIDL,UAT3,UAT4,UAT5,UAT6,UAT7,UAT8,UAT9"/>
    <add key="SitGaiaUrl" value="https://sit-gaia.example.com"/>
    <add key="UatGaiaUrl" value="https://uat-gaia.example.com"/>
    <add key="ProdGaiaUrl" value="https://prod-gaia.example.com"/>
    <add key="DevGaiaUrl" value="https://dev-gaia.example.com"/>
    <add key="PreprodGaiaUrl" value="https://preprod-gaia.example.com"/>
  </appSettings>
</configuration>










https://github.com/dotnet/AspNetDocs/blob/main/aspnet/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md

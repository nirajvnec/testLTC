dotnet restore
dotnet clean
dotnet build



dotnet --list-sdks



weatherforecast


These certificates are not meant for production but are helpful for development and testing purposes to simulate an HTTPS environment.


The dotnet dev-certs https command manages development certificates for running secured sites locally.

public static class HttpInterceptorMiddlewareExtensions
{
    public static IApplicationBuilder UseHttpInterceptor(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<HttpInterceptorMiddleware>();
    }
}



public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseHttpsRedirection();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    // Add your custom middleware here
    app.UseHttpInterceptor();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}



public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // Other middleware...
    app.UseHttpInterceptor();
    // More middleware...
}





using Microsoft.AspNetCore.Http;
using System;
using System.IO;
using System.Threading.Tasks;

public class HttpInterceptorMiddleware
{
    private readonly RequestDelegate _next;

    public HttpInterceptorMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        // Check for bearer token
        var authHeader = context.Request.Headers["Authorization"].ToString();
        if (authHeader.StartsWith("Bearer ", StringComparison.OrdinalIgnoreCase))
        {
            var token = authHeader.Substring("Bearer ".Length).Trim();
            Console.WriteLine($"Bearer token found: {token}");
            
            // TODO: Validate the token here
            // For example:
            // if (!IsValidToken(token))
            // {
            //     context.Response.StatusCode = 401; // Unauthorized
            //     return;
            // }
        }
        else
        {
            Console.WriteLine("No bearer token found in the request.");
        }

        // Capture the original request body
        var originalBodyStream = context.Request.Body;
        var requestBody = await new StreamReader(context.Request.Body).ReadToEndAsync();

        // Log or process the request
        Console.WriteLine($"Request Body: {requestBody}");

        // Replace the body stream so it can be read again in the pipeline
        var memoryStream = new MemoryStream();
        var streamWriter = new StreamWriter(memoryStream);
        await streamWriter.WriteAsync(requestBody);
        await streamWriter.FlushAsync();
        memoryStream.Position = 0;
        context.Request.Body = memoryStream;

        // Create a new response body stream
        var originalResponseBody = context.Response.Body;
        using (var responseBody = new MemoryStream())
        {
            context.Response.Body = responseBody;

            // Call the next middleware in the pipeline
            await _next(context);

            // Log or process the response
            responseBody.Seek(0, SeekOrigin.Begin);
            var responseBodyText = await new StreamReader(responseBody).ReadToEndAsync();
            Console.WriteLine($"Response Body: {responseBodyText}");

            // Copy the response body to the original stream
            responseBody.Seek(0, SeekOrigin.Begin);
            await responseBody.CopyToAsync(originalResponseBody);
        }
    }

    // TODO: Implement token validation logic
    // private bool IsValidToken(string token)
    // {
    //     // Implement your token validation logic here
    //     // Return true if the token is valid, false otherwise
    // }
}








Sure! Here is the content for the README.md file written in Markdown format:

markdown

# .NET Framework 4.5.2 Projects: Console Client and Web API Server

## Step 1: Create the API Server Project

### 1. Open Visual Studio and Create a New Project

1. Choose **ASP.NET Web Application (.NET Framework)**.
2. Name the project `ApiServerWebApi` and select **.NET Framework 4.5.2**.
3. In the next dialog, choose **Web API** and click **OK**.

### 2. Configure the API

#### Install JWT Authentication Packages

Open the Package Manager Console and run the following commands:

```powershell
Install-Package Microsoft.Owin.Host.SystemWeb -Version 3.0.1
Install-Package Microsoft.Owin.Security.Jwt -Version 4.0.4
Install-Package Microsoft.Owin.Security -Version 4.0.4
Install-Package System.IdentityModel.Tokens.Jwt -Version 4.0.4
Add OWIN Startup Class
Add a new class named Startup.cs:

csharp

using System;
using System.Text;
using Microsoft.Owin;
using Microsoft.Owin.Security.Jwt;
using Microsoft.Owin.Security;
using Owin;
using Microsoft.IdentityModel.Tokens;

[assembly: OwinStartup(typeof(ApiServerWebApi.Startup))]
namespace ApiServerWebApi
{
    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            var issuer = "your-issuer";
            var audience = "your-audience";
            var secret = Convert.FromBase64String("your-secret-key");

            app.UseJwtBearerAuthentication(new JwtBearerAuthenticationOptions
            {
                TokenValidationParameters = new TokenValidationParameters
                {
                    ValidIssuer = issuer,
                    ValidAudience = audience,
                    IssuerSigningKey = new SymmetricSecurityKey(secret)
                }
            });
        }
    }
}
Create a Simple API Controller
Add a new controller named ValuesController:

csharp

using System.Web.Http;

namespace ApiServerWebApi.Controllers
{
    [Authorize]
    public class ValuesController : ApiController
    {
        public IHttpActionResult Get()
        {
            return Ok("This is a protected API endpoint.");
        }
    }
}
Configure Web API
In WebApiConfig.cs, ensure you have the following configuration:

csharp

public static class WebApiConfig
{
    public static void Register(HttpConfiguration config)
    {
        config.MapHttpAttributeRoutes();

        config.Routes.MapHttpRoute(
            name: "DefaultApi",
            routeTemplate: "api/{controller}/{id}",
            defaults: new { id = RouteParameter.Optional }
        );
    }
}
Step 2: Create the Console Client Project
1. Create a New Console Application
Open Visual Studio, create a new project, and select Console Application.
Name it ConsoleClient.
2. Implement the Console Client
Install Required Packages
Open the Package Manager Console and run the following commands:

powershell

Install-Package Microsoft.IdentityModel.Tokens -Version 4.0.4
Install-Package System.IdentityModel.Tokens.Jwt -Version 4.0.4
Implement the Token Generation and API Call
Replace the code in Program.cs with the following:

csharp

using System;
using System.IdentityModel.Tokens.Jwt;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Claims;
using Microsoft.IdentityModel.Tokens;
using System.Text;

namespace ConsoleClient
{
    class Program
    {
        static void Main(string[] args)
        {
            var token = GenerateToken();

            CallApi(token).Wait();
        }

        static string GenerateToken()
        {
            var issuer = "your-issuer";
            var audience = "your-audience";
            var secret = Convert.FromBase64String("your-secret-key");

            var tokenHandler = new JwtSecurityTokenHandler();
            var key = new SymmetricSecurityKey(secret);
            var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

            var tokenDescriptor = new SecurityTokenDescriptor
            {
                Subject = new ClaimsIdentity(new[]
                {
                    new Claim(ClaimTypes.Name, "test-user")
                }),
                Expires = DateTime.UtcNow.AddHours(1),
                Issuer = issuer,
                Audience = audience,
                SigningCredentials = credentials
            };

            var token = tokenHandler.CreateToken(tokenDescriptor);
            return tokenHandler.WriteToken(token);
        }

        static async System.Threading.Tasks.Task CallApi(string token)
        {
            using (var client = new HttpClient())
            {
                client.BaseAddress = new Uri("http://localhost:your-port/api/");
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);

                var response = await client.GetAsync("values");
                if (response.IsSuccessStatusCode)
                {
                    var content = await response.Content.ReadAsStringAsync();
                    Console.WriteLine(content);
                }
                else
                {
                    Console.WriteLine("Error: " + response.StatusCode);
                }
            }
        }
    }
}
Replace your-issuer, your-audience, your-secret-key, and your-port with the appropriate values.

Summary
ApiServerWebApi: Hosts a simple protected API endpoint using JWT authentication.
ConsoleClient: Generates a JWT token and calls the protected API endpoint.
Start the API server first and ensure it’s running. Then, run the console client to see the result.



{flowchart}
  "Desktop and Web Applications" -> "Register in GAIA/IDP"
  "Register in GAIA/IDP" -> "GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments."
  "GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments." -> "The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint."
  "The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint." -> "GAIA Endpoint PROD, UAT, SIT"
  "GAIA Endpoint PROD, UAT, SIT" -> "GAIA responds with access token"
  "GAIA responds with access token" -> "The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems."
  "The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems." -> "The API needs to use the access token to retrieve user information such as PID, etc."
{flowchart}




```mermaid
flowchart TD
    A[Desktop and Web Applications] --> B[Register in GAIA/IDP]
    B --> C[GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments.]
    C --> D[The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint. ]
    D --> E{GAIA Endpoint PROD, UAT, SIT}
    E --> F[GAIA responds with access token]
    F --> G[The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems.]
    G --> H[The API needs to use the access token to retrieve user information such as PID, etc.]

Sure, I'll outline the steps to create two .NET Framework 4.5.2 projects: a console client and a web API server.

Step 1: Create the API Server Project
Open Visual Studio and create a new project.
Choose ASP.NET Web Application (.NET Framework).
Name the project ApiServerWebApi and select .NET Framework 4.5.2.
In the next dialog, choose Web API and click OK.
Configure the API
Install JWT Authentication Packages:
Open the Package Manager Console and run the following commands:

powershell

Install-Package Microsoft.Owin.Host.SystemWeb -Version 3.0.1
Install-Package Microsoft.Owin.Security.Jwt -Version 4.0.4
Install-Package Microsoft.Owin.Security -Version 4.0.4
Install-Package System.IdentityModel.Tokens.Jwt -Version 4.0.4
Add OWIN Startup Class:
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
Create a Simple API Controller:
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
Configure Web API:
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
Create a New Console Application:
Open Visual Studio, create a new project, and select Console Application.
Name it ConsoleClient.
Implement the Console Client
Install Required Packages:
Open the Package Manager Console and run the following command:

powershell

Install-Package Microsoft.IdentityModel.Tokens -Version 4.0.4
Install-Package System.IdentityModel.Tokens.Jwt -Version 4.0.4
Implement the Token Generation and API Call:
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
Start the API server first and ensure it’s running. Then, run the console client to see the result





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

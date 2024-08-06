public class UserCheckMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<UserCheckMiddleware> _logger;

    public UserCheckMiddleware(RequestDelegate next, ILogger<UserCheckMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        if (context.User?.Identity?.IsAuthenticated == true)
        {
            _logger.LogInformation("User is authenticated. User name: {UserName}", context.User.Identity.Name);
            
            // You can log more details about claims if needed
            foreach (var claim in context.User.Claims)
            {
                _logger.LogInformation("Claim: {Type} = {Value}", claim.Type, claim.Value);
            }
        }
        else
        {
            _logger.LogInformation("User is not authenticated");
        }

        // Call the next delegate/middleware in the pipeline
        await _next(context);
    }
}


public static class UserCheckMiddlewareExtensions
{
    public static IApplicationBuilder UseUserCheck(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<UserCheckMiddleware>();
    }
}


app.UseAuthentication();
app.UseAuthorization();
app.UseUserCheck(); // Add this line




"start:windows": "ng serve --port 44449 --ssl --ssl-cert %APPDATA%\\ASP.NET\\https\\%npm_package_name%.pem --ssl-key %APPDATA%\\ASP.NET\\https\\%npm_package_name%.key && start msedge --auto-open-devtools-for-tabs https://localhost:44449",



"start:default": "ng serve --port 44449 --ssl --ssl-cert $HOME/.aspnet/https/${npm_package_name}.pem --ssl-key $HOME/.aspnet/https/${npm_package_name}.key && (sleep 5 && open -a \"Microsoft Edge\" --args --auto-open-devtools-for-tabs \"https://localhost:44449\")",

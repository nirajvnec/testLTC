public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // This should come early in the pipeline if the certificate is required for subsequent middlewares
    app.UseMiddleware<CertificateMiddleware>();

    // Authentication Middleware (if you need to authenticate based on the client certificate)
    app.UseAuthentication();

    // Authorization Middleware (if you want to authorize based on the client certificate)
    app.UseAuthorization();

    // Other middlewares (like routing, endpoints, etc.)
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}



public class CertificateMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IClientCertificateService _clientCertificateService;

    public CertificateMiddleware(RequestDelegate next, IClientCertificateService clientCertificateService)
    {
        _next = next;
        _clientCertificateService = clientCertificateService;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var clientCertificate = await context.Connection.GetClientCertificateAsync();

        if (clientCertificate != null)
        {
            _clientCertificateService.Subject = ExtractSubjectFromCertificate(clientCertificate);
            _clientCertificateService.Pid = ExtractPidFromCertificate(clientCertificate);
        }

        await _next(context);
    }

    private string ExtractSubjectFromCertificate(X509Certificate2 certificate)
    {
        // Logic to extract "sub" (subject) from the certificate
        return certificate.Subject;
    }

    private string ExtractPidFromCertificate(X509Certificate2 certificate)
    {
        // Logic to extract Pid (from subject alternative names or other certificate fields)
        var pid = certificate.Subject; // Placeholder, adjust according to actual certificate structure
        
        return pid;
    }
}



public interface IClientCertificateService
{
    string Subject { get; set; }
}

public class ClientCertificateService : IClientCertificateService
{
    public string Subject { get; set; }
}


public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IClientCertificateService, ClientCertificateService>();
}

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // Other middlewares...
    
    app.UseMiddleware<CertificateMiddleware>();

    // Other middlewares...
}

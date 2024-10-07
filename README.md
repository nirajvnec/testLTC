


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
            var subject = clientCertificate.Subject;
            _clientCertificateService.Subject = ExtractSubjectFromCertificate(clientCertificate);
        }

        await _next(context);
    }

    private string ExtractSubjectFromCertificate(X509Certificate2 certificate)
    {
        // Assuming the "sub" (subject) is in the Subject field of the certificate
        var subjectName = certificate.Subject;
        // Extract "sub" from the subject if necessary. Adjust according to your certificate format.
        return subjectName;
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

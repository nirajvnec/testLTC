public async Task InvokeAsync(HttpContext context)
    {
#if DEBUG
        // Hardcoded values for debugging in Visual Studio
        _clientCertificateService.Subject = "This is subject";
        _clientCertificateService.Pid = "This is Pid";
#else
        // Use real client certificate in other environments (non-debug)
        var clientCertificate = await context.Connection.GetClientCertificateAsync();

        if (clientCertificate != null)
        {
            _clientCertificateService.Subject = ExtractSubjectFromCertificate(clientCertificate);
            _clientCertificateService.Pid = ExtractPidFromCertificate(clientCertificate);
        }
#endif

        await _next(context);
    }

using System;
using System.Linq;
using System.Security.Cryptography.X509Certificates;

static void Main(string[] args)
{
    var certificates = GetAllCertificates();
    
    if (certificates != null && certificates.Any())
    {
        Console.WriteLine("Certificate Details:");
        Console.WriteLine("-------------------");

        certificates.Select(cert => new
        {
            IssuedTo = GetCommonName(cert.Subject),
            IssuedBy = GetCommonName(cert.Issuer)
        })
        .ToList() // Convert to List<T> to use ForEach
        .ForEach(info =>
        {
            Console.WriteLine($"Issued to: {info.IssuedTo}");
            Console.WriteLine($"Issued by: {info.IssuedBy}");
            Console.WriteLine("-------------------");
        });
    }
    else
    {
        Console.WriteLine("No certificates found.");
    }

    Console.WriteLine("Hello, World!");
}

private static string GetCommonName(string distinguishedName)
{
    return distinguishedName
        .Split(',')
        .FirstOrDefault(part => part.TrimStart().StartsWith("CN="))
        ?.Substring(3).Trim() ?? "N/A";
}

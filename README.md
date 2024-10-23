using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using System.Security.Cryptography.X509Certificates;
using System.Net.Security;

namespace ConsoleApp38
{
    internal class Program
    {
        static async Task Main(string[] args)
        {
            string url = "https://mars-FT35.csintra.net:30131/CsMarsReportXml/reportxml";
            string xmlContent = @"<?xml version=""1.0""?>
<ROOT>
    <DATA name=""request"">
        <Requests>
            <GetManagedDataItemGroups username=""nkuma152"" type=""Report Definition""/>
        </Requests>
    </DATA>
    <DATA name=""authorisation_token"">
        <MaRSNetSession>
            <LogOnDetails>
                <User name=""nkuma152"">
                    <Groups>
                        <Group name=""MaRS Net Report Control""/>
                    </Groups>
                </User>
            </LogOnDetails>
            <Signature/>
        </MaRSNetSession>
    </DATA>
</ROOT>";

            using (var store = new X509Store(StoreName.My, StoreLocation.CurrentUser))
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByTimeValid, DateTime.Now, true);

                using (var handler = new HttpClientHandler())
                {
                    handler.ClientCertificates.AddRange(col);
                    handler.ServerCertificateCustomValidationCallback = (sender, cert, chain, sslPolicyErrors) => true;

                    using (var client = new HttpClient(handler))
                    {
                        client.Timeout = TimeSpan.FromMilliseconds(3600000);
                        
                        // Replace the StringContent with MultipartFormDataContent
                        var content = new MultipartFormDataContent("CSMarsXMLHttp_MIME_BOUNDARY");
                        content.Add(new StringContent(xmlContent, Encoding.UTF8, "application/xml"), "xmlContent");
                        
                        try
                        {
                            HttpResponseMessage response = await client.PostAsync(url, content);
                            response.EnsureSuccessStatusCode();
                            string responseBody = await response.Content.ReadAsStringAsync();
                            Console.WriteLine("Response:");
                            Console.WriteLine(responseBody);
                        }
                        catch (HttpRequestException e)
                        {
                            Console.WriteLine("Error sending request:");
                            Console.WriteLine(e.Message);
                        }
                    }
                }
            }
        }
    }
}

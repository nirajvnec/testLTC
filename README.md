using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using System.Security.Cryptography.X509Certificates;
using System.Net.Security;
using System.Xml;
using System.IO;
using System.Net;

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

            try
            {
                HttpWebRequest request = WebRequest.Create(url) as HttpWebRequest;
                if (request != null)
                {
                    request.Timeout = 3600000;
                    request.Method = "POST";
                    request.ContentType = "multipart/form-data; boundary=CSMarsXMLHttp_MIME_BOUNDARY";
                    if (url.StartsWith("https:"))
                    {
                        request.ClientCertificates = GetCertificateCollection();
                    }
                }

                byte[] bytes = System.Text.Encoding.UTF8.GetBytes(CreateMimeMessage(xmlContent));
                request.ContentLength = bytes.Length;
                using (Stream requestStream = request.GetRequestStream())
                {
                    requestStream.Write(bytes, 0, bytes.Length);
                }

                using (HttpWebResponse serverResponse = request.GetResponse() as HttpWebResponse)
                {
                    XmlDocument resultDocument = new XmlDocument();
                    using (XmlTextReader responseReader = new XmlTextReader(serverResponse.GetResponseStream()))
                    {
                        resultDocument.Load(responseReader);
                        if (string.IsNullOrEmpty(resultDocument.DocumentElement.OuterXml))
                        {
                            throw new XmlException("<EmptyDefinition>GetManagedDataItemGroups</EmptyDefinition>");
                        }

                        // Check for errors in the response
                        XmlNodeList errorNodes = resultDocument.SelectNodes("/Replies/*[@status='error']//Error | /Reply[@status='error']//Error");
                        if (errorNodes != null && errorNodes.Count > 0)
                        {
                            StringBuilder errorMessage = new StringBuilder();
                            foreach (XmlNode errorNode in errorNodes)
                            {
                                errorMessage.AppendLine(errorNode.InnerText);
                            }
                            throw new ApplicationException($"The XML returned from MARS contains errors: {errorMessage}");
                        }

                        Console.WriteLine("Response processed successfully.");
                    }
                }
            }
            catch (WebException e)
            {
                Console.WriteLine($"Error sending request: {e.Message}");
            }
            catch (XmlException e)
            {
                Console.WriteLine($"Error parsing XML response: {e.Message}");
            }
            catch (ApplicationException e)
            {
                Console.WriteLine(e.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine($"Unexpected error: {e.Message}");
            }
        }

        // Add these methods to your Program class
        private static X509Certificate2Collection GetCertificateCollection()
        {
            var spidCertificate = new X509Certificate2Collection();
            spidCertificate.Import(@"C:\Users\nkuma152\MarsNet_T (S114415) - .pfx", "Pa49stels009igne", 
                X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
            return spidCertificate;
        }

        private static string CreateMimeMessage(string xmlContent)
        {
            string boundary = "CSMarsXMLHttp_MIME_BOUNDARY";
            StringBuilder sb = new StringBuilder();
            sb.AppendLine($"--{boundary}");
            sb.AppendLine("Content-Disposition: form-data; name=\"xmlContent\"");
            sb.AppendLine("Content-Type: application/xml");
            sb.AppendLine();
            sb.AppendLine(xmlContent);
            sb.AppendLine($"--{boundary}--");
            return sb.ToString();
        }
    }
}


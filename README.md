// Static method to get all personal certificates
    public static X509Certificate2Collection GetPersonalCertificates()
    {
        // Create a collection to hold the certificates
        X509Certificate2Collection certCollection = new X509Certificate2Collection();
        
        // Open the "My" store in the Current User context (Personal store)
        using (X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser))
        {
            store.Open(OpenFlags.ReadOnly); // Open the store in read-only mode
            
            // Iterate over the certificates in the store and add them to the collection
            foreach (X509Certificate2 cert in store.Certificates)
            {
                certCollection.Add(cert);
            }
        }

        return certCollection;
    }





using System;
using System.Linq;
using System.Xml.Linq;

namespace CDATAParser
{
    class Program
    {
        static void Main()
        {
            string xml = @"
            <root>
                <data><![CDATA[AssetClass,SA,FileName,UDM Ref Name
                1,2,3,4
                4,5,,6
                ]]></data>
            </root>";

            // Load the XML string into an XDocument
            XDocument doc = XDocument.Parse(xml);

            // Extract the CDATA content
            string cdataContent = doc.Descendants("data").First().Value;

            // Split the content by lines (removing empty entries)
            var rows = cdataContent.Split(Environment.NewLine, StringSplitOptions.RemoveEmptyEntries);

            // LINQ to extract the third column for each row after the header
            var thirdColumnValues = rows.Skip(1) // Skip header
                                        .Select(row => row.Split(','))
                                        .Where(columns => columns.Length >= 3) // Ensure there are at least 3 columns
                                        .Select(columns => string.IsNullOrEmpty(columns[2]) ? "NULL" : columns[2]);

            // Output the third column values
            Console.WriteLine("Third column values:");
            foreach (var value in thirdColumnValues)
            {
                Console.WriteLine(value);
            }
        }
    }
}





public static class StringExtensions
{
    public static string ExtractPid(this string input)
    {
        string pattern = @"\((.*?)\)";
        Match match = Regex.Match(input, pattern);
        return match.Success ? match.Groups[1].Value : string.Empty;
    }
}


string input = "Process (12345) completed";
string pid = input.ExtractPid();

Console.WriteLine(pid);  // Output: 12345

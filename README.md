
using Grpc.Net.Client;
using Grpc.Net.Client.Web;
using System;
using System.Threading.Tasks;

namespace GrpcClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Configure the HttpClient to use gRPC-Web
            var httpClientHandler = new HttpClientHandler();
            var grpcWebHandler = new GrpcWebHandler(GrpcWebMode.GrpcWeb, httpClientHandler);

            // Create a channel to connect to the gRPC service
            var channel = GrpcChannel.ForAddress("http://localhost:5000", new GrpcChannelOptions
            {
                HttpHandler = grpcWebHandler
            });

            // Create a client using the generated client class
            var client = new Greeter.GreeterClient(channel);

            // Call the gRPC service
            var reply = await client.SayHelloAsync(new HelloRequest { Name = "Niraj" });

            // Print the response
            Console.WriteLine("Greeting: " + reply.Message);
        }
    }
}




dotnet add package Microsoft.Office.Interop.Excel


using System;

public static class StringExtensions
{
    public static string ToCapitalizedBoolean(this string input)
    {
        if (bool.TryParse(input, out bool boolValue))
        {
            // Return the capitalized version of the boolean value
            return boolValue.ToString().ToUpper();
        }
        else
        {
            // Return the original string if it's not a valid boolean
            return input;
        }
    }
}




using System;

public static class StringExtensions
{
    public static string ToFormattedDate(this string input)
    {
        if (DateTime.TryParse(input, out DateTime tempDate))
        {
            // Return the date in YYYYMMDD format
            return tempDate.ToString("yyyyMMdd");
        }
        else
        {
            // Return an empty string or any error message
            return string.Empty;
        }
    }
}



using System;
using System.Collections.Generic;
using Microsoft.Office.Interop.Excel;
using System.Runtime.InteropServices;

public List<KeyValuePair<string, string>> PopulateFromExcelRange(Range excelRange)
{
    List<KeyValuePair<string, string>> result = new List<KeyValuePair<string, string>>();

    try
    {
        // Assume the first column is the key and the second column is the value
        for (int row = 1; row <= excelRange.Rows.Count; row++)
        {
            string key = Convert.ToString((excelRange.Cells[row, 1] as Range).Value2);
            string value = Convert.ToString((excelRange.Cells[row, 2] as Range).Value2);

            // Add the key-value pair to the list
            result.Add(new KeyValuePair<string, string>(key, value));
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error reading Excel data: {ex.Message}");
    }
    finally
    {
        // Clean up COM objects
        if (excelRange != null)
            Marshal.ReleaseComObject(excelRange);
    }

    return result;
}
```

To use this method, you would call it like this:

```csharp
// Assuming you have already opened the Excel workbook and worksheet
Worksheet worksheet = (Worksheet)workbook.Worksheets[1];
Range range = worksheet.UsedRange;

List<KeyValuePair<string, string>> myList = PopulateFromExcelRange(range);

// Don't forget to clean up COM objects when you're done
Marshal.ReleaseComObject(worksheet);
Marshal.ReleaseComObject(workbook);
Marshal.ReleaseComObject(excelApp);
```

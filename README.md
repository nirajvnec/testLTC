

@"\((.*?)\)";




string pattern = @"\(M(\d+)\)";



using System;
using System.Text.RegularExpressions;

public static class StringExtensions
{
    // Extension method to extract the value inside (Mxxxxx) format
    public static string ExtractCode(this string input)
    {
        string pattern = @"(M\d+)";
        Match match = Regex.Match(input, pattern);
        
        return match.Success ? match.Groups[1].Value : string.Empty;
    }
}

class Program
{
    static void Main()
    {
        string sub = "CN=niraj.kumar.4 (M850326), OU=st, DC=edit, DC=hedani, DC=net";
        
        // Use the extension method
        string extractedValue = sub.ExtractCode();
        
        Console.WriteLine("Extracted Value: " + extractedValue);
    }
}





"Kindly update my attendance record to reflect my presence, as I attended the session but was mistakenly marked as absent."





Here’s an explanation of why gRPC and the add-in can't coexist within the same project when using .NET Framework.

gRPC has limited support in .NET Framework due to several technical constraints. Specifically:

gRPC requires HTTP/2, which is not natively supported by the .NET Framework. While it can be configured to work using WinHttpHandler, this setup is not ideal for every environment and often leads to issues when integrating with other components.

gRPC is better supported in .NET Core/5+. These versions have more modern HTTP handling capabilities (HTTP/2 by default), along with better asynchronous support, making gRPC integration smoother and more efficient.

Add-ins typically rely on features and libraries specific to .NET Framework, which creates a conflict when trying to integrate gRPC, a feature that works best with .NET Core or later versions. As a result, attempting to use both add-ins and gRPC together in a .NET Framework project leads to compatibility and configuration issues.

For robust gRPC support, we strongly recommend migrating to .NET Core or .NET 5/6, where gRPC is fully supported with fewer limitations and better performance.

Please let me know if further clarification is needed.


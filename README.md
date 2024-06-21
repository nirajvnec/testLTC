PS C:\> $path = $env:PATH -split ';'
PS C:\> $path | ForEach-Object { $_ }

C:\Program Files\nodejs\
C:\Users\YourUsername\AppData\Roaming\npm
C:\Windows\System32
...



($env:PATH -split ';') | Where-Object { $_ -like '*npm*' }



using System;
using System.Reflection;

public class Example
{
    public static void Main()
    {
        try
        {
            var catalogElement = new CatalogElement();
            var methodInfo = typeof(CatalogElement).GetMethod("GetModules", BindingFlags.Instance | BindingFlags.Public | BindingFlags.NonPublic);
            
            if (methodInfo != null)
            {
                var result = methodInfo.Invoke(catalogElement, null);
                Console.WriteLine($"Modules: {string.Join(", ", (string[])result)}");
            }
            else
            {
                Console.WriteLine("Method not found.");
            }
        }
        catch (MethodAccessException ex)
        {
            Console.WriteLine($"MethodAccessException: {ex.Message}");
        }
    }
}

public class CatalogElement
{
    // Make sure this method is accessible
    public string[] GetModules()
    {
        return new string[] { "Module1", "Module2" };
    }
}





Hi Hari,

I recommend raising a JIRA ticket and categorizing it under "Application Features for Improved Operational Perspective." Get the JIRA approved before proceeding.

Alternatively, you can create a proof of concept (POC) to demonstrate the benefits. This approach makes it easier to convince business stakeholders of the advantages.

Once approved, proceed with coding the full feature, testing, and merging it. Pointing out poor implementation during a release call can be counterproductive for the team.

Best regards,
[Your Name]

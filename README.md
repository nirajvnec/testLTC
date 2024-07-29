using System;
using System.DirectoryServices;

public class ActiveDirectoryHelper
{
    public static void GetAllUserProperties(string username)
    {
        try
        {
            // Set the path to the directory entry (Active Directory root)
            DirectoryEntry entry = new DirectoryEntry("LDAP://YourDomainController");
            
            // Create a directory searcher
            DirectorySearcher searcher = new DirectorySearcher(entry);
            
            // Set the filter to search for the specified sAMAccountName
            searcher.Filter = $"(sAMAccountName={username})";
            
            // Perform the search
            SearchResult result = searcher.FindOne();
            
            if (result != null)
            {
                // Iterate through all properties and display their names and values
                foreach (string propertyName in result.Properties.PropertyNames)
                {
                    Console.WriteLine($"Property: {propertyName}");
                    foreach (object propertyValue in result.Properties[propertyName])
                    {
                        Console.WriteLine($"    Value: {propertyValue}");
                    }
                }
            }
            else
            {
                Console.WriteLine("User not found.");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An error occurred: {ex.Message}");
        }
    }
}

// Usage
ActiveDirectoryHelper.GetAllUserProperties("YourUsername");




 have amended my previous access request submission as it was missing some essential access rights. The corrected request has now been submitted and is awaiting your approval.





# Request for Individual Authorizations

| Function | Profile                                      | Authorization                                 |
|----------|----------------------------------------------|-----------------------------------------------|
| ADD      | HSJ-DEV_OBSERVER - OBSERVER READ ONLY ROLE   | AT40377 - MARKET RISK DEFAULT RISK DEV        |
| ADD      | HSJ-DEV_OBSERVER - OBSERVER READ ONLY ROLE   | AT40379 - MARKET RISK DEFAULT RISK PREPROD    |
| ADD      | HSJ-TE_DEVELOP - STANDARD DEVELOPER ROLE     | AT39392 - DevCloud Desktop DEV FRRI           |
| ADD      | HSJ-DEV_DEVELOP - WORKSPA.                   | AT16738                                       |
| ADD      | HSJ-DEV_DEVELOP - WORKSPA.                   | AT39392                                       |
| ADD      | HSJ-DEV_DEVOPS - WORKSPAC.                   | AT40377                                       |

## Period of Validity

* **Valid from**: 29.07.2024
* **Valid to**: 29.07.2026

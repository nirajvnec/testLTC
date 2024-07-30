As a newly joined team member, I request that my email ID be added to the  distribution list(s) to ensure I receive all necessary communications.


Dear [Recipient's Name],

I hope this message finds you well.

I am writing to bring to your attention an inconsistency I have encountered in the authorization system. When selecting the first three dropdowns—System, Authorization Domain, and Operational Environment—the rest of the fields should be automatically populated. However, I have noticed a discrepancy between two different instances.

In the first instance, based on the email screenshot sent by Ganesh Mali:

System: CONFLUENCE WMSB — H2I
Authorization Domain: IT4IT Services — 997
Operational Environment: PRODUCTION
This selection should populate the following fields:
4. Subsystem: IT4IT Services Production — ITS
5. *Profile ( mandatory)**: PROFILE FOR OCE INSTANCE — H2I-CONFL-OCE
6. Authorization (maximum number 999):
- For support personnel ONLY - Admin role — CONFLUENCE-ADMINISTRATOR
- User permission for application access — CONFLUENCE-USER


When I am raising the request today in goto/bbs, I see the following with the same selections for the first three dropdowns:

System: CONFLUENCE WMSB — H2I
Authorization Domain: IT4IT Services — 997
Operational Environment: PRODUCTION
The populated fields are slightly different:
4. Subsystem: IT4IT Services Production — ITS
5. *Profile ( mandatory)**: PROFILE FOR CENTRAL CONFLUENCE — H2I-CONFL-OCE
6. Authorization (maximum number 999):
- For support personnel ONLY - Admin role — CONFLUENCE-ADMINISTRATOR

The field User permission for application access — CONFLUENCE-USER is missing in this instance as well.

Could you please look into this discrepancy and provide guidance on how to resolve it? If additional information is needed, feel free to reach out.

Thank you for your assistance.





using System;
using System.Collections.Generic;
using System.DirectoryServices;

public class ActiveDirectoryHelper
{
    public static Dictionary<string, List<string>> GetAllUserProperties(string username)
    {
        Dictionary<string, List<string>> userProperties = new Dictionary<string, List<string>>();

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
                // Iterate through all properties and add them to the dictionary
                foreach (string propertyName in result.Properties.PropertyNames)
                {
                    List<string> values = new List<string>();
                    foreach (object propertyValue in result.Properties[propertyName])
                    {
                        values.Add(propertyValue.ToString());
                    }
                    userProperties[propertyName] = values;
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

        return userProperties;
    }
}

// Usage
var userProps = ActiveDirectoryHelper.GetAllUserProperties("YourUsername");

// Display the properties
foreach (var property in userProps)
{
    Console.WriteLine($"Property: {property.Key}");
    foreach (var value in property.Value)
    {
        Console.WriteLine($"    Value: {value}");
    }
}


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

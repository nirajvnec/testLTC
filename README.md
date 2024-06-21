# Define a function to build the Angular project with the production configuration
function ngprod {
    ng build --configuration=production
}






# Get the current Path environment variable
$currentPath = [System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::User)

# Get the path of the PowerShell profile script on the network share
$profilePath = "\\NetworkShare\Path\To\Your\Profile\Microsoft.PowerShell_profile.ps1"

# Append the profile path to the current Path variable
$newPath = "$currentPath;$profilePath"

# Set the new Path environment variable
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::User)

# Verify the updated Path environment variable
echo $env:Path











# Get the current Path environment variable
$currentPath = [System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::User)

# Get the path of the PowerShell profile script in OneDrive
$profilePath = "$env:USERPROFILE\OneDrive\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"

# Append the profile path to the current Path variable
$newPath = "$currentPath;$profilePath"

# Set the new Path environment variable
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::User)

# Verify the updated Path environment variable
echo $env:Path










# Get the current Path environment variable
$currentPath = [System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::User)

# Get the path of the PowerShell profile script
$profilePath = $PROFILE

# Append the profile path to the current Path variable
$newPath = "$currentPath;$profilePath"

# Set the new Path environment variable
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::User)

# Verify the updated Path environment variable
echo $env:Path









# Get the path of the PowerShell profile script
$profilePath = $PROFILE

# Add the profile path to the environment variables for the current user
[System.Environment]::SetEnvironmentVariable("POWERSHELL_PROFILE_PATH", $profilePath, [System.EnvironmentVariableTarget]::User)

# Verify the new environment variable
echo $env:POWERSHELL_PROFILE_PATH







Test-Path $PROFILE

New-Item -Path $PROFILE -Type File -Force

notepad $PROFILE

Set-Alias ngprod "ng build --configuration=production"
 

. $PROFILE

ngprod







ng build --configuration=production





# Get the current PATH variable
$oldPath = [System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::User)

# Add the npm path
$newPath = $oldPath + ";C:\Users\YourUsername\AppData\Roaming\npm"

# Set the new PATH variable
[System.Environment]::SetEnvironmentVariable("Path", $newPath, [System.EnvironmentVariableTarget]::User)

# Verify the PATH variable
echo $env:PATH







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

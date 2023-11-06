Please contact with the Production Support Team regarding your current concern. It appears that the problem you are encountering is not associated with the MET application, but rather is specific to your Virtual Machine (VM).

Thank you for your prompt attention to this matter.




Deployment Efficiency: Achieve 95% successful deployments using .NET 6/7 in the Azure environment.

Serverless Response Time: Target sub-200ms response times for serverless .NET 6/7 applications on Azure Functions.

Front-end Load Time: Aim for sub-3-second load times for Angular or React SPAs integrated with .NET 6/7 Web APIs.

Continuous Integration Success: Maintain a 98% success rate for CI/CD pipelines of .NET 6/7 applications using Azure DevOps.

Database Latency: Keep average latency below 150ms for Cosmos DB requests from .NET 6/7 applications.

Real-time Communication: Ensure 99.9% uptime for real-time applications using SignalR with .NET 6/7 on Azure.

Component Reusability: Achieve a 90% rate of component reuse in Angular or React apps integrated with .NET 6/7.

Server-side Rendering Success: Attain 95% successful server-side rendering for Angular Universal or React apps with .NET 6/7.

Authentication Efficiency: Achieve 99.5% successful authentications using Azure AD with .NET 6/7 applications.

Scalability Metric: Ensure .NET 6/7 applications on Azure can handle a 200% increase in user load without degradation in performance.

This version presents the points more as measurable outcomes or goals that one might track as part of project metrics.








Successfully completed the development of Subject Matter Expert (SME) knowledge bases for the Metallurgical (MET), Construction (CN), and Heavy Machinery (HM) sectors.

Addressed and rectified a critical production bug within the MET application that was causing system crashes upon startup.

Ensured stable functionality in the MET system, particularly when users attempt to generate multiple reports simultaneously.


Developed SME Knowledge MET, CN, HM,

Recently fixed Production Critical bug for MET where MET was crashing after launching, When user was trying to Run multiple reports


Earlier, when I entered into the RADAR project, there was no way to execute the Stored Procedure from SQL Developer, which made it challenging to check output in Oracle SQL Developer. Then, I immediately looked for another Oracle client. Where can we execute the stored Procedure and found that AllRound PL/SQL Developer is a better tool that we can use to perform Stored Procedure execution from this client. Which was not possible earlier with Oracle SQL Developer



git checkout feature/MRT_GUI_ET_IRT


git log -1 a9a3e7a3b1d4c2093a5dd8f9b2b0e08726cb5c11^



git show a9a3e7a3b1d4c2093a5dd8f9b2b0e08726cb5c11




git fetch origin
git log
git fetch --all




git fsck --full



git log -1 a9a3e7a3b1d4c2093a5dd8f9b2b0e08726cb5c11




git merge-base remotes/origin/feature/MRT_GUI_ET_IRT remotes/origin/release/MRT_GUI_ET_23.4.0




# Fetch all branches
git fetch

# Get a list of all branches, excluding the specific ones mentioned
$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -notlike '*remotes/origin/HEAD*' -and $_ -notlike '*feature/MRT_GUI_ET_IRT*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    # For diagnostic purposes
    Write-Host "Processing branch: $branch"

    # Try-catch block to handle potential errors
    try {
        $mergeBase = git merge-base refs/remotes/origin/feature/MRT_GUI_ET_IRT $branch
        if ($mergeBase) {
            $date = git show -s --format=%ci $mergeBase
            $possibleOrigins += [PSCustomObject]@{
                Branch   = $branch
                MergeBase= $mergeBase
                Date     = $date
            }
        }
    } catch {
        Write-Host "Error processing branch: $branch"
    }
}

# Sort by date descending
$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

# Now the first item in the sorted list should be the branch created just before feature/MRT_GUI_ET_IRT.
$previousBranch = $sortedOrigins[0].Branch

Write-Host "The branch created just before feature/MRT_GUI_ET_IRT is: $previousBranch"













# Fetch all branches
git fetch

# Get a list of all branches, excluding the specific ones mentioned
$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -notlike '*remotes/origin/HEAD*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    # For diagnostic purposes
    Write-Host "Processing branch: $branch"

    # Try-catch block to handle potential errors
    try {
        $mergeBase = git merge-base refs/remotes/origin/feature/MRT_GUI_ET_IRT $branch
        if ($mergeBase) {
            $date = git show -s --format=%ci $mergeBase
            $possibleOrigins += [PSCustomObject]@{
                Branch   = $branch
                MergeBase= $mergeBase
                Date     = $date
            }
        }
    } catch {
        Write-Host "Error processing branch: $branch"
    }
}

# Sort by date descending
$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

# Assuming the first item in the sorted list is feature/MRT_GUI_ET_IRT, 
# the second item would be the branch created just before it.
$previousBranch = $sortedOrigins[1].Branch

Write-Host "The branch created just before feature/MRT_GUI_ET_IRT is: $previousBranch"
































$sortedOrigins = $possibleOrigins | Sort-Object Date -Ascending







$sortedOrigins[1].Branch



git fetch

$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -ne "*feature/MRT_GUI_ET_IRT*" -and $_ -notlike '*remotes/origin/HEAD*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    # For diagnostic purposes
    Write-Host "Processing branch: $branch"

    # Try-catch block to handle potential errors
    try {
        $mergeBase = git merge-base refs/remotes/origin/feature/MRT_GUI_ET_IRT $branch
        if ($mergeBase) {
            $date = git show -s --format=%ci $mergeBase
            $possibleOrigins += [PSCustomObject]@{
                Branch   = $branch
                MergeBase= $mergeBase
                Date     = $date
            }
        }
    } catch {
        Write-Host "Error processing branch: $branch"
    }
}

$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

$sortedOrigins[0].Branch



git fetch

$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -ne "*feature/MRT_GUI_ET_IRT*" -and $_ -notlike '*remotes/origin/HEAD*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    # For diagnostic purposes
    Write-Host "Processing branch: $branch"

    # Try-catch block to handle potential errors
    try {
        $mergeBase = git merge-base refs/remotes/origin/feature/MRT_GUI_ET_IRT $branch
        if ($mergeBase) {
            $date = git show -s --format=%ci $mergeBase
            $possibleOrigins += [PSCustomObject]@{
                Branch   = $branch
                MergeBase= $mergeBase
                Date     = $date
            }
        }
    } catch {
        Write-Host "Error processing branch: $branch"
    }
}

$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

$sortedOrigins[0].Branch



git fetch

$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -ne "*feature/MRT_GUI_ET_IRT*" -and $_ -notlike '*remotes/origin/HEAD*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    # For diagnostic purposes
    Write-Host "Processing branch: $branch"

    # Try-catch block to handle potential errors
    try {
        $mergeBase = git merge-base refs/remotes/origin/feature/MRT_GUI_ET_IRT $branch
        if ($mergeBase) {
            $date = git show -s --format=%ci $mergeBase
            $possibleOrigins += [PSCustomObject]@{
                Branch   = $branch
                MergeBase= $mergeBase
                Date     = $date
            }
        }
    } catch {
        Write-Host "Error processing branch: $branch"
    }
}

$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

$sortedOrigins[0].Branch






git fetch

$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -ne "*feature/MRT_GUI_ET_IRT*" -and $_ -notlike '*remotes/origin/HEAD*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    $mergeBase = git merge-base refs/remotes/origin/feature/MRT_GUI_ET_IRT $branch
    if ($mergeBase) {
        $date = git show -s --format=%ci $mergeBase
        $possibleOrigins += [PSCustomObject]@{
            Branch   = $branch
            MergeBase= $mergeBase
            Date     = $date
        }
    }
}

$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

$sortedOrigins[0].Branch











git fetch

$branches = git branch -a | ForEach-Object { $_.trim() } | Where-Object { $_ -ne "feature/MRT_GUI_ET_IRT" -and $_ -notlike '*remotes/origin/HEAD*' }

$possibleOrigins = @()

foreach ($branch in $branches) {
    $mergeBase = git merge-base feature/MRT_GUI_ET_IRT $branch
    if ($mergeBase) {
        $date = git show -s --format=%ci $mergeBase
        $possibleOrigins += [PSCustomObject]@{
            Branch   = $branch
            MergeBase= $mergeBase
            Date     = $date
        }
    }
}

$sortedOrigins = $possibleOrigins | Sort-Object Date -Descending

$sortedOrigins[0].Branch







$targetBranch = "feature/MRT_GUI_ET_IRT"
$branches = git branch -r | Where-Object { $_ -notlike "*$targetBranch*" }

foreach ($branch in $branches) {
    $commonCommit = git merge-base $targetBranch $branch.Trim()
    if ($commonCommit -eq (git rev-parse $targetBranch)) {
        Write-Host "$branch branched from $targetBranch at commit $commonCommit"
    }
}



$targetBranch = "feature/MRT_GUI_ET_IRT"
$branches = git branch -r | Where-Object { $_ -notlike "*$targetBranch*" }

foreach ($branch in $branches) {
    $commonCommit = git merge-base $targetBranch $branch.Trim()
    Write-Host "$branch branched from commit $commonCommit"
}



$branches = git branch -a | Where-Object { $_ -notmatch "HEAD" } | ForEach-Object { $_.trim().Replace('*', '').Replace('remotes/origin/', '') }
foreach ($branch in $branches) {
    Write-Host "Checking against $branch"
    git merge-base feature/MRT_GUI_ET_IRT $branch
}




$branches = git branch | Where-Object { $_ -notmatch "HEAD" } | ForEach-Object { $_.trim().Replace('*', '') }
foreach ($branch in $branches) {
    Write-Host "Checking against $branch"
    git merge-base feature/MRT_GUI_ET_IRT $branch
}



$branches = git branch | Where-Object { $_ -notmatch "HEAD" }
foreach ($branch in $branches) {
    Write-Host "Checking against $branch"
    git merge-base feature/MRT_GUI_ET_IRT $branch.trim()
}



$branches = git branch --all | Where-Object { $_ -notmatch "HEAD" }
foreach ($branch in $branches) {
    Write-Host "Checking against $branch"
    git merge-base feature/MRT_GUI_ET_IRT $branch
}



git merge-base feature/MRT_GUI_ET_IRT master


git merge-base feature/MRT_GUI_ET_IRT other_branch



git show e635cc6ea5d63822bd139dbad3ca706927e6a01a



git log master --oneline | findstr e635cc6ea5d63822bd139dbad3ca706927e6a01a

git log master --oneline | grep abc1234

git checkout -b master origin/master
git merge-base feature/MRT_GUI_ET_IRT master




git fetch
git branch -a
git checkout -b master origin/master
git merge-base feature/MRT_GUI_ET_IRT master




git merge-base feature/MRT_GUI_ET_IRT master
git log -1 [COMMIT_HASH]



Demonstrated ownership of tasks and responsibilities.
Managed MET, a desktop-based application.
Oversaw the functioning and maintenance of RADAR application.
Took charge of HIERARCHY MANAGER software.
Successfully handled MARSNET2, another pivotal desktop application.
Ensured smooth operations for the DRC STRATEGIC.
Managed the NON STRATEGIC WORK projects.
Took initiative in R to C# code amendments.
Showed integrity in every responsibility taken up.
Regularly updated and improved applications under management.
Actively sought feedback to enhance application performance.
Collaborated with a team to ensure best practices.
Addressed and resolved any application-related issues promptly.
Provided continuous support for all managed applications.
Demonstrated a proactive approach to problem-solving.
Successfully completed the certified engineer program.
Displayed commitment to ongoing professional development.
Always ensured high-quality deliverables.
Promoted transparency and open communication.
Maintained a consistent record of accountability and reliability.



















First, make sure you're inside your Git repository.

Fetch the latest changes from your remote repository to ensure you're comparing the latest versions of the branches:

sql
Copy code
git fetch origin
Use the git diff command to see the differences between the two branches:
arduino
Copy code
git diff release/MRT_GUI_IRT..feature/MRT_GUI_ET_23.4.0
This will show you the differences between the two branches. If you want to see a list of commits that are present in one branch and not the other, you can use the git log command:

arduino
Copy code
git log release/MRT_GUI_IRT..feature/MRT_GUI_ET_23.4.0
If you want to get these changes into one of the branches, you can either merge or rebase depending on your workflow. Make sure to consult your team's best practices when deciding how to incorporate changes from one branch to another.




User
How to push all commits to feature/GUI_ET_23.4.0

ChatGPT
To push all your local commits to the branch feature/GUI_ET_23.4.0 on the remote repository, follow these steps:

Ensure you're inside your Git repository.

Make sure you've committed all your changes. You can check for uncommitted changes with:

lua
Copy code
git status
If there are any uncommitted changes, commit them:
sql
Copy code
git add .
git commit -m "Your commit message here"
Push your commits to the remote branch. If you're currently on the branch feature/GUI_ET_23.4.0, you can simply do:
perl
Copy code
git push origin feature/GUI_ET_23.4.0
If you're not on the feature/GUI_ET_23.4.0 branch, you'll first need to check out to that branch and then push:

perl
Copy code
git checkout feature/GUI_ET_23.4.0
git push origin feature/GUI_ET_23.4.0
This will push all your local commits to the feature/GUI_ET_23.4.0 branch on the remote repository. If there are any conflicts or if the remote branch has been updated in the meantime, Git will let you know, and you might need to pull the latest changes from the remote branch and resolve any conflicts before you can push your commits.




User
No I am first I need to take it from release/MRT_GUI_IRT and then push to feature/GUI_ET_23.4.0

ChatGPT
Got it! You want to bring changes from release/MRT_GUI_IRT into feature/GUI_ET_23.4.0 and then push them to the remote feature/GUI_ET_23.4.0 branch. Here's a step-by-step guide:

Fetch the latest changes from your remote repository:
sql
Copy code
git fetch origin
Check out the branch feature/GUI_ET_23.4.0:
Copy code
git checkout feature/GUI_ET_23.4.0
Merge (or rebase) the changes from release/MRT_GUI_IRT into feature/GUI_ET_23.4.0:
Using merge:

sql
Copy code
git merge release/MRT_GUI_IRT
OR if you prefer using rebase (this will place your feature/GUI_ET_23.4.0 branch's commits on top of the release/MRT_GUI_IRT branch's commits):

arduino
Copy code
git rebase release/MRT_GUI_IRT
If there are any merge conflicts, you'll need to resolve them manually. Once resolved, you can continue the merge or rebase process:
sql
Copy code
git add [resolved-file]
git commit
For rebase conflicts:

csharp
Copy code
git add [resolved-file]
git rebase --continue
Once everything is merged or rebased cleanly, push your changes to the remote feature/GUI_ET_23.4.0 branch:
perl
Copy code
git push origin feature/GUI_ET_23.4.0
If you're rebasing, especially if you've already pushed feature/GUI_ET_23.4.0 to the remote before, you may need to use a force push since the commit history will have changed:

csharp
Copy code
git push origin feature/GUI_ET_23.4.0 --force-with-lease
Note: Force pushing can be risky if others are working on the same branch, as it overwrites the history. Always communicate with your team when performing such operations.




Send a message


git config --global --unset credential.helper

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.Json;
using System.Threading;
using System.Threading.Tasks;

public interface IUserPermissionFilter
{
    Task<IEnumerable<string>> GetLinksForUserAsync(List<UserPermission> userPermissions, string filePath);
}

public class UserPermissionService : IUserPermissionFilter
{
    private readonly SemaphoreSlim semaphore = new(1, 1);

    private readonly Dictionary<string, Func<UserPermission, string, bool>> _filterMap = new()
    {
        ["READ WRITE"] = (permission, link) => 
        {
            bool marsUploadToolCondition = !(link.Contains("mars-upload-tool/") && !permission.AttributeValue.Contains("MARS_UPLOAD_TOOL"));
            bool marsCrdDslReportCondition = !(link.Contains("mars-crd-dsl-report") && !permission.AttributeValue.Contains("MARSCRDDSLREPORT"));
            return marsUploadToolCondition && marsCrdDslReportCondition;
        },
        ["READ ONLY"] = (permission, link) => 
        {
            return !(link.Contains("/rrp/") && !permission.AttributeValue.Contains("REPORTS_RRP"));
        }
    };

    public async Task<IEnumerable<string>> GetLinksForUserAsync(List<UserPermission> userPermissions, string filePath)
    {
        await semaphore.WaitAsync();
        
        try
        {
            string jsonContent = await File.ReadAllTextAsync(filePath);
            var linkJson = JsonSerializer.Deserialize<Dictionary<string, Dictionary<string, List<Link>>>>(jsonContent);

            var validLinks = new List<string>();
            
            foreach (var userPermission in userPermissions)
            {
                foreach (var right in linkJson["Global"].Keys)
                {
                    if (_filterMap.TryGetValue(right, out var filterFunc))
                    {
                        foreach (var link in linkJson["Global"][right])
                        {
                            if (filterFunc(userPermission, link.Route))
                            {
                                validLinks.Add(link.Route);
                            }
                        }
                    }
                }
            }

            return validLinks.Distinct(); // Removing duplicates as multiple userPermissions might yield the same links.
        }
        finally
        {
            semaphore.Release();
        }
    }
}

public record UserPermission(string TechIDValue, string RightName, string ScopeValueSet, string AttributeName, string AttributeValue);

public record Link(string Route, string Scope, string IsSwissLink, string IsGlobalLink);










using System;
using System.Collections.Generic;
using System.IO;
using System.Text.Json;
using System.Threading;
using System.Threading.Tasks;
using System.Linq;

public interface IUserPermissionFilter
{
    Task<IEnumerable<string>> GetLinksForUserAsync(List<UserPermission> userPermissions, string filePath);
}

public record Link(string Link, string Scope, string IsSwissLink, string IsGlobalLink);
public record UserPermission(string TechIDValue, string RightName, string ScopeValueSet, string AttributeName, string AttributeValue);

public class UserPermissionFilter : IUserPermissionFilter
{
    private static readonly SemaphoreSlim semaphore = new(1);

    private readonly Dictionary<string, Func<UserPermission, string, bool>> _filterMap = new()
    {
        ["READ WRITE"] = (permission, link) =>
        {
            bool marsUploadToolCondition = !(link.Contains("mars-upload-tool/") && !permission.AttributeValue.Contains("MARS_UPLOAD_TOOL"));
            bool marsCrdDslReportCondition = !(link.Contains("mars-crd-dsl-report") && !permission.AttributeValue.Contains("MARSCRDDSLREPORT"));
            return marsUploadToolCondition && marsCrdDslReportCondition;
        },
        ["READ ONLY"] = (permission, link) => 
        {
            return !(link.Contains("/rrp/") && !permission.AttributeValue.Contains("REPORTS_RRP"));
        }
    };

    public async Task<IEnumerable<string>> GetLinksForUserAsync(List<UserPermission> userPermissions, string filePath)
    {
        await semaphore.WaitAsync();
        
        try
        {
            string jsonContent = await File.ReadAllTextAsync(filePath);
            var linkJson = JsonSerializer.Deserialize<Dictionary<string, Dictionary<string, List<Link>>>>(jsonContent);

            var validLinks = new List<string>();
            
            foreach (var userPermission in userPermissions)
            {
                foreach (var right in linkJson["Global"].Keys)
                {
                    if (_filterMap.TryGetValue(right, out var filterFunc))
                    {
                        foreach (var link in linkJson["Global"][right])
                        {
                            if (filterFunc(userPermission, link.Link))
                            {
                                validLinks.Add(link.Link);
                            }
                        }
                    }
                }
            }

            return validLinks.Distinct(); // Removing duplicates as multiple userPermissions might yield the same links.
        }
        finally
        {
            semaphore.Release();
        }
    }
}

// Client code
public async Task ClientCode(IUserPermissionFilter filter)
{
    var userPermissions = new List<UserPermission>
    {
        new UserPermission("someTechId1", "READ WRITE", "someScope1", "someAttribute1", "MARS_UPLOAD_TOOL"),
        new UserPermission("someTechId2", "READ ONLY", "someScope2", "someAttribute2", "REPORTS_RRP")
    };
    
    var links = await filter.GetLinksForUserAsync(userPermissions, "path_to_file.json");

    foreach (var link in links)
    {
        Console.WriteLine(link);
    }
}






using System;
using System.Collections.Generic;
using System.IO;
using System.Text.Json;
using System.Threading;
using System.Threading.Tasks;

public interface IUserPermissionFilter
{
    Task<IEnumerable<string>> GetLinksForUserAsync(UserPermission userPermission, string filePath);
}

public record Link(string Link, string Scope, string IsSwissLink, string IsGlobalLink);
public record UserPermission(string TechIDValue, string RightName, string ScopeValueSet, string AttributeName, string AttributeValue);

public class UserPermissionFilter : IUserPermissionFilter
{
    private static readonly SemaphoreSlim semaphore = new(1);

    private readonly Dictionary<string, Func<UserPermission, string, bool>> _filterMap = new()
    {
        ["READ WRITE"] = (permission, link) =>
        {
            bool marsUploadToolCondition = !(link.Contains("mars-upload-tool/") && !permission.AttributeValue.Contains("MARS_UPLOAD_TOOL"));
            bool marsCrdDslReportCondition = !(link.Contains("mars-crd-dsl-report") && !permission.AttributeValue.Contains("MARSCRDDSLREPORT"));
            return marsUploadToolCondition && marsCrdDslReportCondition;
        },
        ["READ ONLY"] = (permission, link) => 
        {
            return !(link.Contains("/rrp/") && !permission.AttributeValue.Contains("REPORTS_RRP"));
        }
    };

    public async Task<IEnumerable<string>> GetLinksForUserAsync(UserPermission userPermission, string filePath)
    {
        await semaphore.WaitAsync();
        
        try
        {
            string jsonContent = await File.ReadAllTextAsync(filePath);
            var linkJson = JsonSerializer.Deserialize<Dictionary<string, Dictionary<string, List<Link>>>>(jsonContent);

            var validLinks = new List<string>();
            
            foreach (var right in linkJson["Global"].Keys)
            {
                if (_filterMap.TryGetValue(right, out var filterFunc))
                {
                    foreach (var link in linkJson["Global"][right])
                    {
                        if (filterFunc(userPermission, link.Link))
                        {
                            validLinks.Add(link.Link);
                        }
                    }
                }
            }

            return validLinks;
        }
        finally
        {
            semaphore.Release();
        }
    }
}

// Client code
public async Task ClientCode(IUserPermissionFilter filter)
{
    var userPermission = new UserPermission("someTechId", "READ WRITE", "someScope", "someAttribute", "MARS_UPLOAD_TOOL");
    var links = await filter.GetLinksForUserAsync(userPermission, "path_to_file.json");

    foreach (var link in links)
    {
        Console.WriteLine(link);
    }
}












using System;
using System.Collections.Generic;
using System.Linq;
using System.IO;
using Newtonsoft.Json;

public record UserPermission(
    string TechIDValue,
    string RightName,
    string ScopeValueSet,
    string AttributeName,
    string AttributeValue
);

public interface ILinkRepository
{
    Dictionary<string, Dictionary<string, List<Dictionary<string, string>>>> GetAllLinks();
}

public class JsonLinkRepository : ILinkRepository
{
    private readonly string _filePath;

    public JsonLinkRepository(string filePath)
    {
        _filePath = filePath;
    }

    public Dictionary<string, Dictionary<string, List<Dictionary<string, string>>>> GetAllLinks()
    {
        var jsonString = File.ReadAllText(_filePath);
        return JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, List<Dictionary<string, string>>>>>(jsonString);
    }
}

public interface ILinkFilter
{
    IEnumerable<string> FilterLinks(UserPermission permission, IEnumerable<Dictionary<string, string>> links);
}

public class PermissionBasedLinkFilter : ILinkFilter
{
    private readonly Dictionary<string, Func<UserPermission, string, bool>> _filterMap = new()
    {
        ["READ WRITE"] = (permission, link) => 
        {
            return !(!permission.AttributeValue.Contains("MARS_UPLOAD_TOOL") && link.Contains("mars-upload-tool/"));
        }
    };

    public IEnumerable<string> FilterLinks(UserPermission permission, IEnumerable<Dictionary<string, string>> links)
    {
        if (_filterMap.TryGetValue(permission.RightName, out var filterFunc))
        {
            return links.Where(link => filterFunc(permission, link["link"])).Select(link => link["link"]);
        }
        
        return links.Select(link => link["link"]);
    }
}

public class Program
{
    public static void Main()
    {
        ILinkRepository linkRepo = new JsonLinkRepository("links.json");
        ILinkFilter linkFilter = new PermissionBasedLinkFilter();
        
        var allLinks = linkRepo.GetAllLinks();
        List<UserPermission> permissions = GetPermissions();

        var validRights = new[] 
        {
            "READ WRITE"  
        };

        var filteredLinks = permissions
            .Where(p => validRights.Contains(p.RightName))
            .SelectMany(p => linkFilter.FilterLinks(p, allLinks["Global"][p.RightName]));

        foreach (var link in filteredLinks)
        {
            Console.WriteLine(link);
        }
    }

    private static List<UserPermission> GetPermissions()
    {
        // Mocked data for example
        return new List<UserPermission>
        {
            new UserPermission("SomeValue", "READ WRITE", "SomeScope", "SomeAttribute", "SomeValue")
        };
    }
}







using System.Xml;
using System.Collections.Generic;
using System.Linq;

public static class StringExtensions
{
    public static XmlDocument FilterXmlByMultipleCategories(this string xmlString, List<string> categories)
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xmlString);

        XmlDocument newDoc = new XmlDocument();
        XmlElement root = newDoc.CreateElement("Replies");
        newDoc.AppendChild(root);

        foreach (XmlNode node in doc.GetElementsByTagName("Attribute"))
        {
            var categoryAttribute = node.Attributes["category"];
            if (categoryAttribute != null && categories.Contains(categoryAttribute.Value))
            {
                XmlNode importedNode = newDoc.ImportNode(node, true);
                root.AppendChild(importedNode);
            }
        }

        return newDoc;
    }
}














public interface IThreadSafeJsonReader
{
    List<string> GetValuesFromKeys(string filePath, params string[] keys);
}












class Program
{
    static void Main()
    {
        IJsonService jsonService = new JsonService();
        List<string> values = jsonService.GetValuesFromKeys("path_to_your_file.json", "2ND LEVEL SUPPORT READ WRITE", "READ WRITE");

        foreach (var value in values)
        {
            Console.WriteLine(value);
        }
    }
}









using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.IO;

public class JsonService : IJsonService
{
    public List<string> GetValuesFromKeys(string filePath, params string[] keys)
    {
        string json = File.ReadAllText(filePath);
        JObject obj = JObject.Parse(json);
        
        List<string> values = new List<string>();
        foreach (var key in keys)
        {
            if (obj["Global"][key] != null)
            {
                values.AddRange(obj["Global"][key].ToObject<List<string>>());
            }
        }
        
        return values;
    }
}






namespace YourNamespace;

public interface IJsonService
{
    Task<List<string>> GetValuesFromKeysAsync(string filePath, params string[] keys);
}









using System.Xml;





public static class StringExtensions
{
    public static XmlDocument FilterXmlByCategory(this string xmlString, string category)
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xmlString);

        XmlDocument newDoc = new XmlDocument();
        XmlElement root = newDoc.CreateElement("Replies");
        newDoc.AppendChild(root);

        foreach (XmlNode node in doc.GetElementsByTagName("Attribute breakdown"))
        {
            if (node.Attributes["category"] != null && node.Attributes["category"].Value == category)
            {
                XmlNode importedNode = newDoc.ImportNode(node, true);
                root.AppendChild(importedNode);
            }
        }

        return newDoc;
    }
}



using System.Xml;

public static class StringExtensions
{
    public static XmlDocument FilterXmlByCategory(this string xmlString, string category)
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xmlString);

        XmlDocument newDoc = new XmlDocument();
        XmlElement root = newDoc.CreateElement("Replies");
        newDoc.AppendChild(root);

        // XPath query to select "Attribute breakdown" elements with the specified category
        XmlNodeList nodes = doc.SelectNodes($"//*[local-name()='Attribute breakdown' and @category='{category}']");

        foreach (XmlNode node in nodes)
        {
            XmlNode importedNode = newDoc.ImportNode(node, true);
            root.AppendChild(importedNode);
        }

        return newDoc;
    }
}



XmlNodeList nodes = doc.SelectNodes("//*[local-name()='Attribute breakdown']");



using System.Xml;

public static class StringExtensions
{
    public static XmlDocument FilterXmlByCategory(this string xmlString, string category)
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xmlString);

        XmlDocument newDoc = new XmlDocument();
        XmlElement root = newDoc.CreateElement("Replies");
        newDoc.AppendChild(root);

        // Corrected XPath query
        XmlNodeList nodes = doc.SelectNodes($"//*[local-name()='Attribute breakdown' and @category='{category}']");

        foreach (XmlNode node in nodes)
        {
            XmlNode importedNode = newDoc.ImportNode(node, true);
            root.AppendChild(importedNode);
        }

        return newDoc;
    }
}






XmlNodeList nodes = doc.SelectNodes($"//Attribute breakdown[@category='{category}']");




using System.Xml;

public static class StringExtensions
{
    public static XmlDocument FilterXmlByCategory(this string xmlString, string category)
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xmlString);

        XmlDocument newDoc = new XmlDocument();
        XmlElement root = newDoc.CreateElement("Replies");
        newDoc.AppendChild(root);

        // Select all nodes with the specified category
        XmlNodeList nodes = doc.SelectNodes($"//Attribute_breakdown[@category='{category}']");

        foreach (XmlNode node in nodes)
        {
            XmlNode importedNode = newDoc.ImportNode(node, true);
            root.AppendChild(importedNode);
        }

        return newDoc;
    }
}



string xmlString = @"<Replies> ... </Replies>";
XmlDocument filteredDoc = xmlString.FilterXmlByCategory("OTHERS");








<button onClick={() => fetchPost(1)}>Fetch Post</button>

import axios, { AxiosResponse } from 'axios';

interface Post {
    userId: number;
    id: number;
    title: string;
    body: string;
}

async function getPost(id: number): Promise<void> {
    try {
        const url: string = `https://jsonplaceholder.typicode.com/posts/${id}`;
        const response: AxiosResponse<Post> = await axios.get<Post>(url);
        console.log(response.data);
    } catch (error) {
        console.error('There was an error!', error);
    }
}

getPost(1); // Fetch post with ID 1
    





body: string;



import axios from 'axios';

interface IData {
  // Define the shape of the data you expect to receive from the API
  id: number;
  name: string;
  // Add more fields as needed
}

async function fetchData(): Promise<void> {
  try {
    const response = await axios.get<IData>('https://api.example.com/data');
    console.log('Data:', response.data);
  } catch (error) {
    console.error('Error:', error);
  }
}

fetchData();


// If you're using Node.js, first require Axios
// const axios = require('axios');

// Making a GET request to an API endpoint
axios.get('https://api.example.com/data')
  .then(response => {
    // Handle success
    console.log('Data:', response.data);
  })
  .catch(error => {
    // Handle error
    console.error('Error:', error);
  })
  .finally(() => {
    // Always executed
    console.log('Request completed');
  });




<div className="d-flex justify-content-center">
    <button className="btn btn-secondary me-2" onClick={() => setStep(1)}>
        Back
    </button>
    <button className="btn btn-primary" onClick={() => setStep(3)}>
        Next
    </button>
</div>


var filteredAttributes = doc.Descendants("Attribute")
            .Where(x => (string)x.Attribute("category") == filterCategory)
            .ToList();

        XDocument filteredDoc = new XDocument(
            new XElement("Replies",
                new XElement("Attributes", filteredAttributes)));

        // Save the filtered XML document
        filteredDoc.Save(filteredXmlFilePath);




| Column Name  | Data Type   | Allow Nulls |
|--------------|-------------|-------------|
| EmployeeID   | INT         | No          |
| FirstName    | NVARCHAR    | Yes         |
| LastName     | NVARCHAR    | No          |
| DateOfBirth  | DATETIME    | Yes         |
| Position     | NVARCHAR    | Yes         |





| Column Name   | Data Types     | Allow Nulls |
|---------------|----------------|-------------|
| EmpID         | INT            | No          |
| FName         | NVARCHAR(50)   | Yes         |
| LName         | NVARCHAR(50)   | No          |
| DOB           | DATE           | Yes         |
| Job_Position  | NVARCHAR(50)   | Yes         |




using System;
using System.Linq;
using System.Xml.Linq;
using OfficeOpenXml;
using System.IO;

class Program
{
    static void Main()
    {
        string filePath = "path/to/your/xmlfile.xml";  // Replace with the actual file path
        string excelFilePath = "path/to/your/excelfile.xlsx";  // Replace with the desired Excel file path

        XDocument doc;
        try
        {
            doc = XDocument.Load(filePath);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error reading the XML file: {ex.Message}");
            return;
        }

        var attributesByCategory = doc.Descendants("Attribute")
            .GroupBy(x => (string)x.Attribute("category"))
            .Select(g => new AttributeByCategory
            {
                Category = g.Key,
                Name = string.Join(", ", g.Select(x => (string)x.Attribute("name"))),
                Count = g.Count()
            }).ToList();

        // Create Excel file
        using (ExcelPackage package = new ExcelPackage(new FileInfo(excelFilePath)))
        {
            ExcelWorksheet worksheet = package.Workbook.Worksheets.Add("Attributes by Category");

            // Headers
            worksheet.Cells[1, 1].Value = "Category";
            worksheet.Cells[1, 2].Value = "Names";
            worksheet.Cells[1, 3].Value = "Count";

            // Data
            int row = 2;
            foreach (var attr in attributesByCategory)
            {
                worksheet.Cells[row, 1].Value = attr.Category;
                worksheet.Cells[row, 2].Value = attr.Name;
                worksheet.Cells[row, 3].Value = attr.Count;
                row++;
            }

            package.Save();
        }

        Console.WriteLine("Data written to Excel file successfully!");
    }

    public class AttributeByCategory
    {
        public string Category { get; set; }
        public string Name { get; set; }
        public int Count { get; set; }
    }
}




using System;
using System.Collections.Generic;
using System.Linq;
using System.Xml.Linq;

class Program
{
    static void Main()
    {
        string filePath = "path/to/your/xmlfile.xml"; // Replace with the actual file path

        XDocument doc;
        try
        {
            doc = XDocument.Load(filePath);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error reading the XML file: {ex.Message}");
            return;
        }

        var attributesByCategory = doc.Descendants("Attribute")
            .GroupBy(x => (string)x.Attribute("category"))
            .Select(g => new AttributeByCategory
            {
                Category = g.Key,
                Name = string.Join(", ", g.Select(x => (string)x.Attribute("name"))),
                Count = g.Count()
            }).ToList();

        foreach (var attr in attributesByCategory)
        {
            Console.WriteLine($"Category: {attr.Category}, Names: {attr.Name}, Count: {attr.Count}");
        }
    }

    public class AttributeByCategory
    {
        public string Category { get; set; }
        public string Name { get; set; }
        public int Count { get; set; }
    }
}






namespace YourNamespace
{
    partial class FrmFilter
    {
        /// <summary>
        /// Required designer variable.
        /// </summary>
        private System.ComponentModel.IContainer components = null;

        /// <summary>
        /// Clean up any resources being used.
        /// </summary>
        /// <param name="disposing">true if managed resources should be disposed; otherwise, false.</param>
        protected override void Dispose(bool disposing)
        {
            if (disposing && (components != null))
            {
                components.Dispose();
            }
            base.Dispose(disposing);
        }

        #region Windows Form Designer generated code

        /// <summary>
        /// Required method for Designer support - do not modify
        /// the contents of this method with the code editor.
        /// </summary>
        private void InitializeComponent()
        {
            this.cboRole = new System.Windows.Forms.ComboBox();
            this.cboCalculationMethod = new System.Windows.Forms.ComboBox();
            this.cboBreakDownCategory = new System.Windows.Forms.ComboBox();
            this.btnProceed = new System.Windows.Forms.Button();
            this.lblRole = new System.Windows.Forms.Label();
            this.lblCalculationMethod = new System.Windows.Forms.Label();
            this.lblBreakDownCategory = new System.Windows.Forms.Label();
            this.SuspendLayout();
            // 
            // cboRole
            // 
            this.cboRole.FormattingEnabled = true;
            this.cboRole.Location = new System.Drawing.Point(16, 35);
            this.cboRole.Name = "cboRole";
            this.cboRole.Size = new System.Drawing.Size(260, 28);
            this.cboRole.TabIndex = 0;
            // 
            // cboCalculationMethod
            // 
            this.cboCalculationMethod.FormattingEnabled = true;
            this.cboCalculationMethod.Location = new System.Drawing.Point(16, 79);
            this.cboCalculationMethod.Name = "cboCalculationMethod";
            this.cboCalculationMethod.Size = new System.Drawing.Size(260, 28);
            this.cboCalculationMethod.TabIndex = 1;
            // 
            // cboBreakDownCategory
            // 
            this.cboBreakDownCategory.FormattingEnabled = true;
            this.cboBreakDownCategory.Location = new System.Drawing.Point(16, 123);
            this.cboBreakDownCategory.Name = "cboBreakDownCategory";
            this.cboBreakDownCategory.Size = new System.Drawing.Size(260, 28);
            this.cboBreakDownCategory.TabIndex = 2;
            // 
            // btnProceed
            // 
            this.btnProceed.Enabled = false;
            this.btnProceed.Location = new System.Drawing.Point(100, 157);
            this.btnProceed.Name = "btnProceed";
            this.btnProceed.Size = new System.Drawing.Size(100, 35);
            this.btnProceed.TabIndex = 3;
            this.btnProceed.Text = "Proceed";
            this.btnProceed.UseVisualStyleBackColor = true;
            // 
            // lblRole
            // 
            this.lblRole.AutoSize = true;
            this.lblRole.Location = new System.Drawing.Point(12, 12);
            this.lblRole.Name = "lblRole";
            this.lblRole.Size = new System.Drawing.Size(40, 20);
            this.lblRole.TabIndex = 4;
            this.lblRole.Text = "Role";
            // 
            // lblCalculationMethod
            // 
            this.lblCalculationMethod.AutoSize = true;
            this.lblCalculationMethod.Location = new System.Drawing.Point(12, 56);
            this.lblCalculationMethod.Name = "lblCalculationMethod";
            this.lblCalculationMethod.Size = new System.Drawing.Size(147, 20);
            this.lblCalculationMethod.TabIndex = 5;
            this.lblCalculationMethod.Text = "Calculation Method";
            // 
            // lblBreakDownCategory
            // 
            this.lblBreakDownCategory.AutoSize = true;
            this.lblBreakDownCategory.Location = new System.Drawing.Point(12, 100);
            this.lblBreakDownCategory.Name = "lblBreakDownCategory";
            this.lblBreakDownCategory.Size = new System.Drawing.Size(159, 20);
            this.lblBreakDownCategory.TabIndex = 6;
            this.lblBreakDownCategory.Text = "Break Down Category";
            // 
            // FrmFilter
            // 
            this.AutoScaleDimensions = new System.Drawing.SizeF(9F, 20F);
            this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
            this.BackColor = System.Drawing.Color.LightGray;
            this.ClientSize = new System.Drawing.Size(300, 204);
            this.Controls.Add(this.lblBreakDownCategory);
            this.Controls.Add(this.lblCalculationMethod);
            this.Controls.Add(this.lblRole);
            this.Controls.Add(this.btnProceed);
            this.Controls.Add(this.cboBreakDownCategory);
            this.Controls.Add(this.cboCalculationMethod);
            this.Controls.Add(this.cboRole);
            this.Name = "FrmFilter";
            this.Text = "Filter";
            this.ResumeLayout(false);
            this.PerformLayout();
        }

        #endregion

        private System.Windows.Forms.ComboBox cboRole;
        private System.Windows.Forms.ComboBox cboCalculationMethod;
        private System.Windows.Forms.ComboBox cboBreakDownCategory;
        private System.Windows.Forms.Button btnProceed;
        private System.Windows.Forms.Label lblRole;
        private System.Windows.Forms.Label lblCalculationMethod;
        private System.Windows.Forms.Label lblBreakDownCategory;
    }
}





namespace YourNamespace
{
    partial class FrmFilter
    {
        /// <summary>
        /// Required designer variable.
        /// </summary>
        private System.ComponentModel.IContainer components = null;

        /// <summary>
        /// Clean up any resources being used.
        /// </summary>
        /// <param name="disposing">true if managed resources should be disposed; otherwise, false.</param>
        protected override void Dispose(bool disposing)
        {
            if (disposing && (components != null))
            {
                components.Dispose();
            }
            base.Dispose(disposing);
        }

        #region Windows Form Designer generated code

        /// <summary>
        /// Required method for Designer support - do not modify
        /// the contents of this method with the code editor.
        /// </summary>
        private void InitializeComponent()
        {
            this.cboRole = new System.Windows.Forms.ComboBox();
            this.cboCalculationMethod = new System.Windows.Forms.ComboBox();
            this.cboBreakDownCategory = new System.Windows.Forms.ComboBox();
            this.SuspendLayout();
            // 
            // cboRole
            // 
            this.cboRole.FormattingEnabled = true;
            this.cboRole.Location = new System.Drawing.Point(12, 12);
            this.cboRole.Name = "cboRole";
            this.cboRole.Size = new System.Drawing.Size(260, 28);
            this.cboRole.TabIndex = 0;
            // 
            // cboCalculationMethod
            // 
            this.cboCalculationMethod.FormattingEnabled = true;
            this.cboCalculationMethod.Location = new System.Drawing.Point(12, 46);
            this.cboCalculationMethod.Name = "cboCalculationMethod";
            this.cboCalculationMethod.Size = new System.Drawing.Size(260, 28);
            this.cboCalculationMethod.TabIndex = 1;
            // 
            // cboBreakDownCategory
            // 
            this.cboBreakDownCategory.FormattingEnabled = true;
            this.cboBreakDownCategory.Location = new System.Drawing.Point(12, 80);
            this.cboBreakDownCategory.Name = "cboBreakDownCategory";
            this.cboBreakDownCategory.Size = new System.Drawing.Size(260, 28);
            this.cboBreakDownCategory.TabIndex = 2;
            // 
            // FrmFilter
            // 
            this.AutoScaleDimensions = new System.Drawing.SizeF(9F, 20F);
            this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
            this.BackColor = System.Drawing.Color.LightGray;
            this.ClientSize = new System.Drawing.Size(284, 121);
            this.Controls.Add(this.cboBreakDownCategory);
            this.Controls.Add(this.cboCalculationMethod);
            this.Controls.Add(this.cboRole);
            this.Name = "FrmFilter";
            this.Text = "Filter";
            this.ResumeLayout(false);
        }

        #endregion

        private System.Windows.Forms.ComboBox cboRole;
        private System.Windows.Forms.ComboBox cboCalculationMethod;
        private System.Windows.Forms.ComboBox cboBreakDownCategory;
    }
}




using System;
using System.Drawing;
using System.Windows.Forms;
using System.Drawing.Drawing2D;
using System.Threading.Tasks;

namespace YourNamespace
{
    public partial class FrmFilter : Form
    {
        public FrmFilter()
        {
            InitializeComponent();

            // Add Selection Change event handlers
            this.cboRole.SelectedIndexChanged += async (sender, e) => await CboRole_SelectedIndexChanged(sender, e);
            this.cboCalculationMethod.SelectedIndexChanged += async (sender, e) => await CboCalculationMethod_SelectedIndexChanged(sender, e);
            this.cboBreakDownCategory.SelectedIndexChanged += async (sender, e) => await CboBreakDownCategory_SelectedIndexChanged(sender, e);
        }

        private async Task CboRole_SelectedIndexChanged(object sender, EventArgs e)
        {
            // Do something asynchronously when the Role combo box selection changes
            await Task.Delay(500); // Example delay - replace with actual async operations
            Console.WriteLine("Role Selected Index Changed");
        }

        private async Task CboCalculationMethod_SelectedIndexChanged(object sender, EventArgs e)
        {
            // Do something asynchronously when the Calculation Method combo box selection changes
            await Task.Delay(500); // Example delay - replace with actual async operations
            Console.WriteLine("Calculation Method Selected Index Changed");
        }

        private async Task CboBreakDownCategory_SelectedIndexChanged(object sender, EventArgs e)
        {
            // Do something asynchronously when the Break Down Category combo box selection changes
            await Task.Delay(500); // Example delay - replace with actual async operations
            Console.WriteLine("Break Down Category Selected Index Changed");
        }

        protected override void OnPaint(PaintEventArgs e)
        {
            LinearGradientBrush linearGradientBrush = 
                new LinearGradientBrush(this.ClientRectangle, Color.LightSkyBlue, Color.LightGray, 45F);

            e.Graphics.FillRectangle(linearGradientBrush, this.ClientRectangle);

            base.OnPaint(e);
        }
    }
}




recentReportDefinitionsToolStripMenuItem.DropDownItems.Clear();

for (int i = 0; i < Math.Min(current_recent_docs_limit, items.Keys.Count); i++) 
{
    if (items["file" + Convert.ToString(items.Keys.Count - i, 10)] != null) 
    { 
        menu_index++;

        ToolStripItem subItem = new ToolStripMenuItem(Convert.ToString(menu_index, 10) + " " + items["file" + Convert.ToString(items.Keys.Count - i, 10)]);
        subItem.Tag = items["file" + Convert.ToString(items.Keys.Count - i, 10)]; 
        recentReportDefinitionsToolStripMenuItem.DropDownItems.Add(subItem); 

        subItem.Click += async (sender, e) => 
        { 
            await file_recent_docs_submenu_Click(sender, e); 
        };
    } 
}




using System;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Linq;

public class YourClass // NOTE: Replace with the actual name of your class
{
    private HashSet<string> cachedSensitivities = new HashSet<string>();
    private HashSet<string> cachedPositionAttributes = new HashSet<string>();

    public void SetRiskTypeAttributes(Specialized.StringCollection attributes)
    {
        CsAttributeList positionAttributes = globalCache.AttributesDoc.GetPositionAttributes();
        CsBreakdownCollection sensitivityBreakdowns = CsBreakdownCollection.SensitivityAttributeBreakdowns(attributes);

        var currentSensitivities = new HashSet<string>(sensitivityBreakdowns.Cast<CsBreakdown>().Select(b => b.DisplayText.ToUpper()));

        var newSensitivities = currentSensitivities.Except(cachedSensitivities).ToList();

        for (int i = 0; i < newSensitivities.Count; i++)
        {
            var breakdown = sensitivityBreakdowns.Cast<CsBreakdown>().FirstOrDefault(b => b.DisplayText.ToUpper() == newSensitivities[i]);
            if (breakdown != null)
            {
                AddDragDropBreakdownItem(breakdown);
                cachedSensitivities.Add(newSensitivities[i]);
            }
        }

        CsBreakdownCollection positionAttributeBreakdowns = CsBreakdownCollection.AttributeBreakdowns(positionAttributes);

        var currentPositionAttributes = new HashSet<string>(positionAttributeBreakdowns.Cast<CsBreakdown>().Select(b => b.DisplayText.ToUpper()));

        var newPositionAttributes = currentPositionAttributes.Except(cachedPositionAttributes).ToList();

        for (int i = 0; i < newPositionAttributes.Count; i++)
        {
            var breakdown = positionAttributeBreakdowns.Cast<CsBreakdown>().FirstOrDefault(b => b.DisplayText.ToUpper() == newPositionAttributes[i]);
            if (breakdown != null)
            {
                AddDragDropBreakdownItem(breakdown);
                cachedPositionAttributes.Add(newPositionAttributes[i]);
            }
        }

        var attrList = positionAttributeBreakdowns.Cast<CsBreakdown>()
                                                .Select(breakdown => globalCache.AttributesDoc.GetAttribute(breakdown.AttributeName))
                                                .ToList();

        for (int i = 0; i < attrList.Count; i++)
        {
            attributes.Add(attrList[i].AttributeName);
        }

        this.RemoveMissingAttributeBreakdowns(attributes);
    }

    // Other methods and members of your class...

    // Sample implementation of AddDragDropBreakdownItem, replace with your actual implementation
    private void AddDragDropBreakdownItem(CsBreakdown breakdown)
    {
        // Your implementation here
    }

    // Sample implementation of RemoveMissingAttributeBreakdowns, replace with your actual implementation
    private void RemoveMissingAttributeBreakdowns(Specialized.StringCollection attributes)
    {
        // Your implementation here
    }

    // Placeholder for CsAttributeList, Cs





private async Task AddBreakDownsAsync(CtlDragDropBreakdown breakdown, ConcurrentBag<CtlDragDropBreakdown> breakdownBag, bool ignoreValidation = false) 
{
    try 
    {
        // Assuming AddAsync is an existing async method on breakdownBag
        await Task.Run(() => breakdownBag.Add(breakdown));
    } 
    catch (Exception ex) 
    {
        CsMarsErrorHelper.GetInstance().ShowError(ex, "Error adding breakdown");
    }
}






foreach (CtlDragDropItem drag_drop_item in m_drag_drop_items)
{
    if (drag_drop_item == null)
    {
        continue; // skip to the next iteration if drag_drop_item is null
    }

    if (drag_drop_item.Header == null || current_heading == null)
    {
        continue; // skip to the next iteration if Header or current_heading is null
    }

    if ((drag_drop_item.Header.ToUpper() == current_heading.ToUpper() ||
         (current_heading == CsBreakDownHeading.HEADING_ALL + CsBreakDownHeading.HEADING_SUFFIX)) &&
        !drag_drop_item.Disabled && 
        !drag_drop_item.IsDraggedAway &&
        drag_drop_item.Header.ToString() != CsBreakDownHeading.HEADING_VARANALYSIS + CsBreakDownHeading.HEADING_SUFFIX &&
        drag_drop_item.Header.ToString() != CsBreakDownHeading.HEADING_IRCANALYSIS + CsBreakDownHeading.HEADING_SUFFIX)
    {
        drag_drop_item.Top = label_top;
        label_top += SIZE_ITEM_SPACING + drag_drop_item.Height; // assuming this is what you intended
        // item_found_in_header = true;  // uncomment and assign this variable appropriately
        drag_drop_item.Visible = true; // assuming there is a Visible property
    }
    else
    {
        // add appropriate handling for the else case
    }
}
 



private async Task AddBreakdownsToReportAsync(CtlDragDropList.DragDropItemCollection breakdownitems, CS.Mars.ReportDef.CsTableFormat table, CsReportDef.TableDimensionEnum dimension, CsReportDef report_def, CsETGlobalCache global_cache, bool checkScenarioExpression, bool isBreakdownHidden = false)
{ 
    foreach (CtlDragDropBreakdown drag_drop_breakdown in breakdownitems) 
    { 
        drag_drop_breakdown.Breakdown.IsBreakdownHidden = isBreakdownHidden ? "true" : null; 
        
        if (checkScenarioExpression && drag_drop_breakdown.Data is CsBreakdownScenarioExpression) 
        { 
            await drag_drop_breakdown.Breakdown.PopulateReportDefAsync(report_def, table, dimension, global_cache); 
        } 
        else if (!checkScenarioExpression && !(drag_drop_breakdown.Data is CsBreakdownScenarioExpression)) 
        { 
            if (drag_drop_breakdown.Breakdown.WantDisplayName) 
            { 
                if (drag_drop_breakdown.Breakdown.WantNodeNameDisplayName) 
                {
                    // I think some code is missing here, as the if statement is empty
                }
            } 
            
            drag_drop_breakdown.Breakdown.WantDisplayName = false; 
            string subreport_name = drag_drop_breakdown.Breakdown.SubreportName; 
            drag_drop_breakdown.Breakdown.SubreportName = subreport_name + "_NODE_NAME"; 
            
            await drag_drop_breakdown.Breakdown.PopulateReportDefAsync(report_def, table, dimension, global_cache); 
            
            drag_drop_breakdown.Breakdown.WantDisplayName = true; // assuming t_h_ was a typo and meant to reset WantDisplayName
            drag_drop_breakdown.Breakdown.SubreportName = subreport_name; 
        } 
        else if (drag_drop_breakdown.Data is CsBreakdownCobDate && report_def.CalculationMethod is CsCalcMethod) 
        { 
            report_def.BreakdownLevel = dimension.ToString();
            continue; 
        } 
        
        await drag_drop_breakdown.Breakdown.PopulateReportDefAsync(report_def, table, dimension, global_cache);
    } 
}




try 
{
    var context = new ApplicationContext(new FrmMain());
    Application.ThreadException += new ThreadExceptionEventHandler(Application_ThreadException);
    Application.Run(context);
} 
catch (Exception ex) 
{
    Console.WriteLine(ex.Message); // Print the exception message to the console, or handle it as needed
}





public void UpdateUI(Control new_item)
{
    if (new_item == null || label_area_panel == null)
    {
        return;
    }

    void AddControl()
    {
        label_area_panel.Controls.Add(new_item);
    }

    if (label_area_panel.InvokeRequired)
    {
        label_area_panel.BeginInvoke((MethodInvoker)delegate { AddControl(); });
    }
    else
    {
        AddControl();
    }
}



using System;
using System.Collections.Concurrent;
using System.Collections.Specialized;
using System.Linq;
using System.Threading.Tasks;
using System.Windows.Forms;

// ... Other necessary using directives and namespace, class definitions...

public class YourControlClass : Control // Replace with your actual base class
{
    private Label item_label = new Label();

    public override string Text 
    { 
        get 
        {
            if (this.InvokeRequired)
            {
                return (string)this.Invoke(new Func<string>(() => this.Text));
            }
            else
            {
                return item_label.Text;
            }
        }
        set 
        {
            if (this.InvokeRequired)
            {
                this.Invoke(new Action<string>((s) => this.Text = s), value);
            }
            else
            {
                item_label.Text = value;
            }
        }
    }
}

// ... The rest of your previous code ...






using System.Collections.Concurrent;
using System.Collections.Specialized;
using System.Linq;
using System.Threading.Tasks;

// ... Other necessary using directives and namespace, class definitions...

public void SetRiskTypeAttributes(StringCollection attributes)
{
    // Assuming this is thread-safe or making it thread-safe is needed.
    CsAttributeList positionAttributes = globalCache.AttributesDoc.GetPositionAttributes();

    CsBreakdownCollection positionAttributeBreakdowns = CsBreakdownCollection.AttributeBreakdowns(positionAttributes);
    
    // Using a ConcurrentBag to ensure thread-safety
    ConcurrentBag<CtlDragDropItem> safeListItems = new ConcurrentBag<CtlDragDropItem>(ListItems.Cast<CtlDragDropItem>());

    var tempPositionBreakdown = positionAttributeBreakdowns.Cast<CsBreakdown>()
                                                           .Select(b => b.DisplayText)
                                                           .Except(safeListItems.Select(i => i.Text.ToUpper()))
                                                           .ToArray();

    if (tempPositionBreakdown.Any())
    {
        var safePositionBreakdowns = new ConcurrentBag<CsBreakdown>(positionAttributeBreakdowns.Cast<CsBreakdown>()
                                                                                              .Where(b => tempPositionBreakdown.Contains(b.DisplayText)));

        Parallel.ForEach(safePositionBreakdowns, breakdown =>
        {
            AddDragDropBreakdownItem(breakdown); // Ensure this method is thread-safe
        });
    }
    
    CsBreakdownCollection sensitivityBreakdowns = CsBreakdownCollection.SensitivityAttributeBreakdowns(attributes);

    var tempSensitivity = sensitivityBreakdowns.Cast<CsBreakdown>()
                                               .Select(b => b.DisplayText)
                                               .Except(safeListItems.Select(i => i.DisplayText))
                                               .ToArray();

    if (tempSensitivity.Any())
    {
        var safeSensitivityBreakdowns = new ConcurrentBag<CsBreakdown>(sensitivityBreakdowns.Cast<CsBreakdown>()
                                                                                           .Where(b => tempSensitivity.Contains(b.DisplayText)));
        
        Parallel.ForEach(safeSensitivityBreakdowns, breakdown =>
        {
            AddDragDropBreakdownItem(breakdown); // Ensure this method is thread-safe
        });
    }

    // Added new section with thread-safety
    var safeAttributes = new ConcurrentBag<string>();
    Parallel.ForEach(positionAttributeBreakdowns.Cast<CsBreakdown>(), breakdown =>
    {
        var attribute = globalCache.AttributesDoc.GetAttribute(breakdown.AttributeName);
        safeAttributes.Add(attribute.AttributeName);
    });

    // Corrected this line to use the attributes parameter
    RemoveMissingAttributeBreakdowns(attributes); 
    attributes.AddRange(safeAttributes.ToArray());
}

// Make sure the methods RemoveMissingAttributeBreakdowns and AddDragDropBreakdownItem are thread-safe or are made thread-safe









public void UpdateUI(Control new_item)
{
    if (label_area_panel.InvokeRequired)
    {
        label_area_panel.BeginInvoke((MethodInvoker)delegate 
        {
            label_area_panel.Controls.Add(new_item);
        });
    }
    else
    {
        label_area_panel.Controls.Add(new_item);
    }
}




using System;
using System.Collections.Concurrent;
using System.Linq;

public class CtlDragDropBreakdown
{
    public Breakdown Breakdown { get; set; }
}

public class Breakdown
{
    public string DisplayText { get; set; }
}

public class BreakdownManager
{
    private readonly ConcurrentBag<CtlDragDropBreakdown> items = new ConcurrentBag<CtlDragDropBreakdown>();

    public bool FindBreakdownByName(string breakdownAttributeName)
    {
        var breakdown = items.FirstOrDefault(existingItem =>
            existingItem.Breakdown.DisplayText.Equals(breakdownAttributeName, StringComparison.OrdinalIgnoreCase));
        return breakdown != null;
    }

    public void AddBreakdown(CtlDragDropBreakdown breakdown)
    {
        items.Add(breakdown);
    }

    // Potentially other methods to modify and access the 'items' collection
}




private readonly ReaderWriterLockSlim rwLock = new ReaderWriterLockSlim();

public bool FindBreakdownByName(string breakdownAttributeName)
{
    this.rwLock.EnterReadLock();
    CtlDragDropBreakdown[] snapshot;
    try
    {
        snapshot = this.Cast<CtlDragDropBreakdown>().ToArray();
    }
    finally
    {
        this.rwLock.ExitReadLock();
    }

    var breakdown = snapshot.FirstOrDefault(existingItem =>
        existingItem.Breakdown.DisplayText.Equals(breakdownAttributeName, StringComparison.OrdinalIgnoreCase));
    
    return breakdown != null;
}

if (this.lbl_area_panel.InvokeRequired)
{
    this.lbl_area_panel.Invoke((MethodInvoker)delegate
    {
        this.lbl_area_panel.Controls.Add(new_item);
    });
}
else
{
    this.lbl_area_panel.Controls.Add(new_item);
}







private async Task UpdateAvailableAttributesAsync()
{
    try
    {
        CsAttributeList attributeList; // Assuming the initialization or declaration elsewhere
        StringCollection riskTypes = other_filter_grid.Empty 
            ? other_filter_grid.GetRiskTypes(1) 
            : other_filter_grid.GetRiskTypes();

        var riskTypeValidation = new RiskTypeValidation(m_report_def_helper, riskTypes);

        string errorMsg = riskTypeValidation.GetCommonAttributesForRiskTypes(
            other_filter_grid.IcEditMode,
            cob_datepicker.Value,
            m_global_cache.IsDstRefMethodSelected,
            ref attributeList);

        riskTypeValidation.AddSensitivityAttributeIfContainScenario(attributeList, m_global_cache);
        StringCollection attributeNames = attributeList.AttributeNames;

        if (!this.InvokeRequired)
        {
            if (!DslMarsSwitch.IsRefMethodDisabled)
            {
                ctlCalcReportFormat.SetRiskTypeAttributes(attributeNames);
                ctlCalcReportFormat.SetSearched();
            }
        }
        else
        {
            if (MslMarsSwitch.IsRefMethodDisabled)
            {
                ctlCalcReportFormat.SetRiskTypeAttributes(attributeNames);
                ctlCalcReportFormat.SetSearched();
            }

            await Task.Run(() =>
            {
                if (!Ds1MarsSwitch.IsRefMethodDisabled)
                    ctlCalcReportFormat.SetRiskTypeAttributes(attributeNames);
            });
        }
    }
    catch (Exception ex)
    {
        // Handle exception here
        Console.WriteLine(ex.Message); // Replace with actual exception handling
    }
}




public partial class YourForm : Form 
{
    public YourForm()
    {
        InitializeComponent();
        this.openToolStripMenuItem.Click += async (sender, e) => await FileOpenMenuAsync(sender, e);
    }

    private async Task FileOpenMenuAsync(object sender, EventArgs e)
    {
        OpenFileDialog openFileDialog = new OpenFileDialog();

        if (openFileDialog.ShowDialog() == DialogResult.OK) 
        {
            string fileName = openFileDialog.FileName;

            // Assuming that ReadFileContentAsync is a method that reads file content asynchronously
            string content = await ReadFileContentAsync(fileName);

            // Do something with the content...
        }
    }

    private async Task<string> ReadFileContentAsync(string fileName) 
    {
        using (StreamReader reader = new StreamReader(fileName))
        {
            return await reader.ReadToEndAsync();
        }
    }
}




private void InitializeBusyIndicator()
{
    // Initialize panel
    panelBusyIndicator = new Panel
    {
        Size = new Size(200, 100),
        BackColor = Color.FromArgb(50, 255, 255, 224), // semi-transparent yellow-white
        Visible = false
    };

    panelBusyIndicator.Paint += PanelBusyIndicator_Paint; // Add this line

    // The rest of your code remains the same...
}

// Add this method to your form’s code
private void PanelBusyIndicator_Paint(object sender, PaintEventArgs e)
{
    Control control = sender as Control;
    if (control != null)
    {
        using (Pen pen = new Pen(Color.Red, 2)) // Set the border color and width here
        {
            e.Graphics.DrawRectangle(pen, 0, 0, control.Width - 1, control.Height - 1);
        }
    }
}







public partial class MainForm : Form
{
    private Panel panelBusyIndicator;
    private Label lblPleaseWait;

    public MainForm()
    {
        InitializeComponent();
        InitializeBusyIndicator();
    }

    private void InitializeBusyIndicator()
    {
        // Initialize panel
        panelBusyIndicator = new Panel
        {
            Size = new Size(200, 100),
            BackColor = Color.FromArgb(128, 0, 0, 0), // semi-transparent black
        };

        // Initialize label with "Please wait..." text
        lblPleaseWait = new Label
        {
            Text = "Please wait...",
            ForeColor = Color.White, // making text visible against semi-transparent black background
            AutoSize = false,
            Size = panelBusyIndicator.Size,
            TextAlign = ContentAlignment.MiddleCenter,
        };

        panelBusyIndicator.Controls.Add(lblPleaseWait);
        this.Controls.Add(panelBusyIndicator);
    }

    private async void btnLoadData_Click(object sender, EventArgs e)
    {
        panelBusyIndicator.BringToFront(); // bring panel to the front

        panelBusyIndicator.Location = new Point(
            (this.ClientSize.Width - panelBusyIndicator.Width) / 2,
            (this.ClientSize.Height - panelBusyIndicator.Height) / 2);

        panelBusyIndicator.Visible = true;
        panelBusyIndicator.Enabled = false;

        await Task.Run(() => LoadData());

        panelBusyIndicator.Visible = false;
    }

    private void LoadData()
    {
        System.Threading.Thread.Sleep(5000);
    }
}







public partial class MainForm : Form
{
    private Panel panelBusyIndicator;
    private Label lblPleaseWait;

    public MainForm()
    {
        InitializeComponent();
        InitializeBusyIndicator();
    }

    private void InitializeBusyIndicator()
    {
        // Initialize panel
        panelBusyIndicator = new Panel
        {
            Size = new Size(200, 100), // Adjust size as needed
            BackColor = Color.FromArgb(128, 255, 255, 255), // Semi-transparent white
            Visible = false
        };

        // Initialize label
        lblPleaseWait = new Label
        {
            Text = "Please wait...",
            AutoSize = true,
            Location = new Point((panelBusyIndicator.Width - lblPleaseWait.Width) / 2, 
                                 (panelBusyIndicator.Height - lblPleaseWait.Height) / 2)
        };

        // Add label to panel
        panelBusyIndicator.Controls.Add(lblPleaseWait);

        // Add panel to form
        this.Controls.Add(panelBusyIndicator);
    }

    private async void btnLoadData_Click(object sender, EventArgs e)
    {
        // Position the busy indicator panel in the center of the MainForm
        panelBusyIndicator.Location = new Point(
            (this.ClientSize.Width - panelBusyIndicator.Width) / 2,
            (this.ClientSize.Height - panelBusyIndicator.Height) / 2);

        // Show busy indicator
        panelBusyIndicator.Visible = true;
        panelBusyIndicator.Enabled = false; // Disables the panel so the form is still interactive

        // Simulate a long-running operation
        await Task.Run(() => LoadData());

        // Hide busy indicator
        panelBusyIndicator.Visible = false;
    }

    private void LoadData()
    {
        // Simulate a delay to represent a long-running task.
        System.Threading.Thread.Sleep(5000);
    }
}




this.Load += async (sender, e) => await Form_Loaded(sender, e);

public partial class FrmLoadingReportsIndicator : Form
{
    private System.Windows.Forms.Label lblStatus = new System.Windows.Forms.Label();
    private System.Windows.Forms.ProgressBar progressBar = new System.Windows.Forms.ProgressBar();

    public FrmLoadingReportsIndicator()
    {
        InitializeComponent();
        this.StartPosition = FormStartPosition.CenterParent;
    }

    private void InitializeComponent()
    {
        this.lblStatus.Location = new System.Drawing.Point(12, 9);
        this.lblStatus.Name = "lblStatus";
        this.lblStatus.Size = new System.Drawing.Size(260, 23);
        this.lblStatus.TabIndex = 0;
        this.lblStatus.Text = "Status";

        this.progressBar.Location = new System.Drawing.Point(12, 35);
        this.progressBar.Name = "progressBar";
        this.progressBar.Size = new System.Drawing.Size(260, 23);
        this.progressBar.TabIndex = 1;

        this.AutoScaleDimensions = new System.Drawing.SizeF(6F, 13F);
        this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
        this.ClientSize = new System.Drawing.Size(284, 70);
        this.Controls.Add(this.progressBar);
        this.Controls.Add(this.lblStatus);
        this.Name = "FrmLoadingReportsIndicator";
        this.Text = "Loading...";
    }

    public void UpdateStatus(string text, int progress)
    {
        lblStatus.Text = text;
        progressBar.Value = progress;
    }
}




private void LoadReportDef(string fileName)
{
    Task.Run(async () => 
    {
        CsReportDef report_def = await OpenReportDefFromFileAsync(fileName);

        if (report_def.CalculationMethod != null)
        {
            if (report_def.CalculationMethod.SupportsWhatIf && 
                !CsSessionData.GetInstance().IsErcNtcsUserConsent)
            {
                if (m_global_cache != null && 
                    m_global_cache.ConfigDoc != null)
                {
                    ctlCalcReportFormat.ShowErcNtcsMessage(m_global_cache.ConfigDoc.ErcNtcsLdprHelpLink);
                }
            }
        }

        CsRegistryHelper.SetOption("DefaultFolder", Path.GetDirectoryName(fileName));
        DslMarsSwitch.CalculationFormatReset(ctlCalcReportFormat.GetSelectedMethod(), report_def.CalculationMethod); // Renamed here
        ctlCalcReportFormat.ResetDerivationCriteria();
        UpdateRecentDocRegistry(fileName, RECENT_DOCS_REG);

        try
        {
            m_busy_form = new FrmBusyStatus(backgroundWorker, report_def);
            m_busy_form.Status = "Loading report...";
            await m_busy_form.ShowDialogAsync(this); // Assuming there's an async version
            CsSessionData.GetInstance().LoadedReportName = 
                string.IsNullOrWhiteSpace(report_def.ReportFileName) ? 
                report_def.ReportName : 
                report_def.ReportFileName;
            Text = MARS_ENQUIRY_TOOL_STUB + "- " + CsSessionData.GetInstance().LoadedReportName;
        }
        catch (Exception ex)
        {
            CsReportRunException exception = new CsReportRunException(ex.Message, ex, report_def.ToString(), report_def.ReportName);
            CsMarsErrorHelper.GetInstance().ShowError(exception, "Error populating from report definition", false);
        }
    });
}




sub.Click += async (sender, e) => await file_recent_docs_submenu_click(sender, e);



private async Task LoadReportDefAsync(string fileName)
{
    CsReportDef report_def = await OpenReportDefFromFileAsync(fileName);

    if (report_def.CalculationMethod != null)
    {
        if (report_def.CalculationMethod.SupportsWhatIf && 
            !CsSessionData.GetInstance().IsErcNtcsUserConsent)
        {
            if (m_global_cache != null && 
                m_global_cache.ConfigDoc != null)
            {
                ctlCalcReportFormat.ShowErcNtcsMessage(m_global_cache.ConfigDoc.ErcNtcsLdprHelpLink);
            }
        }
    }

    CsRegistryHelper.SetOption("DefaultFolder", Path.GetDirectoryName(fileName));
    Ds1MarsSwitch.CalculationFormatReset(ctlCalcReportFormat.GetSelectedMethod(), report_def.CalculationMethod);
    ctlCalcReportFormat.ResetDerivationCriteria();
    UpdateRecentDocRegistry(fileName, RECENT_DOCS_REG);

    try
    {
        m_busy_form = new FrmBusyStatus(backgroundWorker, report_def);
        m_busy_form.Status = "Loading report...";
        await m_busy_form.ShowDialogAsync(this); // Assuming there's an async version
        CsSessionData.GetInstance().LoadedReportName = 
            string.IsNullOrWhiteSpace(report_def.ReportFileName) ? 
            report_def.ReportName : 
            report_def.ReportFileName;
        Text = MARS_ENQUIRY_TOOL_STUB + "- " + CsSessionData.GetInstance().LoadedReportName;
    }
    catch (Exception ex)
    {
        CsReportRunException exception = new CsReportRunException(ex.Message, ex, report_def.ToString(), report_def.ReportName);
        CsMarsErrorHelper.GetInstance().ShowError(exception, "Error populating from report definition", false);
    }
}





private async Task<CsReportDef> OpenReportDefFromFileAsync(string filePath)
{
    CsReportDef report_def;

    Cursor.Current = Cursors.WaitCursor;

    try
    {
        using (StreamReader stream = new StreamReader(filePath))
        {
            XmlDocument xml_doc = new XmlDocument();
            string content = await stream.ReadToEndAsync();
            xml_doc.LoadXml(content);

            // Reset other parameters as needed before creating CsReportDef
            resetOtherParameters();

            report_def = new CsReportDef(m_report_def_helper)
            {
                ReportFileName = Path.GetFileName(filePath)
            };

            report_def.LoadXml(xml_doc);
            AddPropertyTypeSetsToCache(report_def);
        }
    }
    catch (XmlException ex)
    {
        throw new ApplicationException($"The file {filePath} contains invalid xml", ex);
    }

    return report_def;
}





Hi Santosh, 
This is not a realistic scenario. It is the limitation of the application. If the User Object Count reaches 10000 and the Handle count also goes more than 12500 32-bit application, Please create a release note that this is the limitation beyond which the Windows form application will crash.


using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

public static class StringExtensions
{
    public static string InsertSpaceAfterRead(this string input)
    {
        if (string.IsNullOrEmpty(input))
            return input;

        if (input.StartsWith("READ"))
        {
            return "READ " + input.Substring(4);
        }
        return input;
    }
}

public static class ListExtensions
{
    public static List<string> TransformList(this List<string> list)
    {
        return list.Select(str => str.InsertSpaceAfterRead()).ToList();
    }
}

public class Program
{
    public static void Main()
    {
        var strings = new List<string> {"READRUN", "READONLY", "READWRITE"};
        
        var transformedStrings = strings.TransformList();
        
        foreach(var str in transformedStrings)
        {
            Console.WriteLine(str);
        }
    }
}

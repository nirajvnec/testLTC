$SolutionName = "MaRSRiskServerGateway"
$projectNames = @("Core", "UI", "Tests")

# Create main directory
New-Item -ItemType Directory -Force -Path $SolutionName
Set-Location $SolutionName

# Create solution file
dotnet new sln -n $SolutionName

# Create projects
foreach ($project in $projectNames) {
    $projectPath = "src\$SolutionName.$project"
    New-Item -ItemType Directory -Force -Path $projectPath
    
    Set-Location $projectPath
    
    switch ($project) {
        "UI" { dotnet new wpf }
        "Tests" { dotnet new xunit }
        default { dotnet new classlib }
    }
    
    Set-Location -Path ..\..\..
}

# Add projects to solution
foreach ($project in $projectNames) {
    dotnet sln add "src\$SolutionName.$project\$SolutionName.$project.csproj"
}

# Add project references
dotnet add "src\$SolutionName.UI\$SolutionName.UI.csproj" reference "src\$SolutionName.Core\$SolutionName.Core.csproj"
dotnet add "src\$SolutionName.Tests\$SolutionName.Tests.csproj" reference "src\$SolutionName.Core\$SolutionName.Core.csproj"
dotnet add "src\$SolutionName.Tests\$SolutionName.Tests.csproj" reference "src\$SolutionName.UI\$SolutionName.UI.csproj"

# Install NuGet packages
dotnet add "src\$SolutionName.Core\$SolutionName.Core.csproj" package ClosedXML
dotnet add "src\$SolutionName.UI\$SolutionName.UI.csproj" package Unity

# Create additional folders
$additionalFolders = @(
    "src\$SolutionName.Core\Interfaces",
    "src\$SolutionName.Core\Models",
    "src\$SolutionName.Core\Services",
    "src\$SolutionName.UI\ViewModels",
    "src\$SolutionName.UI\Views",
    "src\$SolutionName.Tests\CoreTests",
    "src\$SolutionName.Tests\UITests"
)

foreach ($folder in $additionalFolders) {
    New-Item -ItemType Directory -Force -Path $folder
}

# Create README.md
New-Item -ItemType File -Force -Path "README.md"
Set-Content -Path "README.md" -Value "# $SolutionName`n`nThis is a Risk Server Gateway application for MaRS."

Write-Host "Project structure for $SolutionName has been set up successfully!"

# SetupMaRSRiskServerGateway.ps1

function Setup-MaRSRiskServerGateway {
    $SolutionName = "MaRSRiskServerGateway"
    $projectNames = @("Core", "UI", "Tests")
    $baseDir = "C:\Users\nkuma152\$SolutionName"

    # Create base directory
    New-Item -ItemType Directory -Force -Path $baseDir | Out-Null
    Set-Location $baseDir

    # Create solution file
    dotnet new sln -n $SolutionName

    # Create projects
    foreach ($project in $projectNames) {
        $projectPath = Join-Path $baseDir "src\$SolutionName.$project"
        New-Item -ItemType Directory -Force -Path $projectPath | Out-Null
        
        Set-Location $projectPath
        
        switch ($project) {
            "UI" { dotnet new wpf }
            "Tests" { dotnet new xunit }
            default { dotnet new classlib }
        }
        
        Set-Location $baseDir
    }

    # Add projects to solution
    foreach ($project in $projectNames) {
        $projectFile = Join-Path $baseDir "src\$SolutionName.$project\$SolutionName.$project.csproj"
        dotnet sln add $projectFile
    }

    # Add project references
    $uiProject = Join-Path $baseDir "src\$SolutionName.UI\$SolutionName.UI.csproj"
    $coreProject = Join-Path $baseDir "src\$SolutionName.Core\$SolutionName.Core.csproj"
    $testsProject = Join-Path $baseDir "src\$SolutionName.Tests\$SolutionName.Tests.csproj"

    dotnet add $uiProject reference $coreProject
    dotnet add $testsProject reference $coreProject
    dotnet add $testsProject reference $uiProject

    # Install NuGet packages
    dotnet add $coreProject package ClosedXML
    dotnet add $uiProject package Unity

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
        New-Item -ItemType Directory -Force -Path (Join-Path $baseDir $folder) | Out-Null
    }

    # Create README.md
    $readmePath = Join-Path $baseDir "README.md"
    New-Item -ItemType File -Force -Path $readmePath | Out-Null
    Set-Content -Path $readmePath -Value "# $SolutionName`n`nThis is a Risk Server Gateway application for MaRS."

    Write-Host "Project structure for $SolutionName has been set up successfully in $baseDir!"
}

function New-MaRSProject {
    $projectPath = "C:\Users\nkuma152\MaRSRiskServerGateway"
    
    # Check if the project already exists
    if (Test-Path $projectPath) {
        Write-Host "A MaRS Risk Server Gateway project already exists at $projectPath"
        $confirmation = Read-Host "Do you want to delete the existing project and create a new one? (Y/N)"
        if ($confirmation -eq 'Y') {
            Remove-Item -Path $projectPath -Recurse -Force
            Write-Host "Existing project deleted."
        } else {
            Write-Host "Operation cancelled. Existing project was not modified."
            return
        }
    }
    
    # Run the setup function
    Setup-MaRSRiskServerGateway
    
    Write-Host "MaRS Risk Server Gateway project structure has been set up in $projectPath"
}

Set-Alias -Name newmars -Value New-MaRSProject

# Uncomment the line below if you want the script to automatically run New-MaRSProject when executed
# New-MaRSProject

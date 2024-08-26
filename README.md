function New-MaRSProject {
    $scriptPath = "C:\Users\nkuma152\SetupMaRSRiskServerGateway.ps1"
    $projectPath = "C:\Users\nkuma152\MaRSRiskServerGateway"
    
    if (Test-Path $scriptPath) {
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

        # Create the directory if it doesn't exist
        New-Item -ItemType Directory -Force -Path $projectPath | Out-Null
        
        # Navigate to the project directory
        Set-Location -Path $projectPath
        
        # Run the script
        & $scriptPath
        
        Write-Host "MaRS Risk Server Gateway project has been set up in $projectPath"
    } else {
        Write-Error "Setup script not found at $scriptPath"
    }
}

Set-Alias -Name newmars -Value New-MaRSProject

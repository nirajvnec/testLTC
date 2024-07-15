# Path to the log file
$logFilePath = "$env:USERPROFILE\LastSelectedCert.log"

# Function to get the last selected certificate
function Get-LastSelectedCertificate {
    if (Test-Path -Path $logFilePath) {
        $thumbprint = Get-Content -Path $logFilePath
        $cert = Get-ChildItem -Path Cert:\CurrentUser\My | Where-Object { $_.Thumbprint -eq $thumbprint }
        return $cert
    } else {
        Write-Host "No previous certificate selection found."
        return $null
    }
}

# Function to display the last selected certificate details
function Show-LastSelectedCertificate {
    $lastCert = Get-LastSelectedCertificate
    if ($lastCert) {
        Write-Host "Last selected certificate details:"
        $lastCert | Format-List
    } else {
        Write-Host "No certificate found or no previous selection recorded."
    }
}

# Alias for displaying the last selected certificate
Set-Alias -Name ViewLastCert -Value Show-LastSelectedCertificate


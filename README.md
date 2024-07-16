import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class EnvironmentParserService {
  private environment: string = '';

  parseEnvironment(hostname: string): void {
    const lowerHostname = hostname.toLowerCase();

    if (lowerHostname.includes('mars-frtb-pte')) {
      this.environment = 'UAT';
    } else if (lowerHostname.includes('mars-frtb')) {
      this.environment = 'PRODUCTION';
    } else {
      // Existing logic for other environments
      const sitEnvironments = ['FT', 'LT', 'ERC'];
      const uatEnvironments = ['UAT', 'PTE'];

      const sitMatch = sitEnvironments.find(env => new RegExp(`\\b${env.toLowerCase()}\\b`).test(lowerHostname));
      const uatMatch = uatEnvironments.find(env => new RegExp(`\\b${env.toLowerCase()}\\b`).test(lowerHostname));

      if (sitMatch) {
        this.environment = 'SIT';
      } else if (uatMatch) {
        this.environment = 'UAT';
      } else if (lowerHostname.includes('localhost')) {
        this.environment = 'DEV';
      } else {
        this.environment = 'PRODUCTION';
      }
    }
  }

  getEnvironment(): string {
    return this.environment;
  }
}





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


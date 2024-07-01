Prerequisites for Teams Convenience Recording:
In order to be able to record a Teams meeting, both the meeting organizer and the person starting the recording (if they are different people) need to meet the following criteria:

Authorization Requirements:

Have one of the following authorizations approved in BBS:
System: AZTS - AZURE TEAMS SERVICE
Sub System: HOS - Service Center Global Production
Profile: AZTS-END USER - ADDITIONAL FEATURES AND FUNCTIONALITY
Authorization Options (one of the four options below):

Internal Conf. + Recording - Conv. Recording only; no External Conf.
External Conf. + Recording - Conv. Recording and External Conf.
Internal Conf. + Recording + DT Sharing - Record + DT Share; no External Conf.
External Conf. + Recording + DT Sharing - Record and DT Share and External Conf.
Click here to learn more about Teams entitlements.

Mode Requirements:

Be a user of Teams Island Mode or Teams Only Mode (learn more at goto/Teams).
BBS Role Approval:

Once your BBS role is approved, please sign out of your Microsoft Teams application:
Click the profile icon at the top-right of the application.
Select Sign out.
Close your Microsoft Teams application, then re-open it and sign in again.


It appears that the Docker image required for the build could not be found, which caused the build to fail during the "Publish Artifactory" stage.

However, when we retriggered the build, it was able to find the image docker-win.odyssey.apps.csintra.net/com/csg/caas/images/windows/build/vsbuildtools2017-15.9.13-office:4.7.2-dotnet-framework-10.0.14393.3506 successfully. This suggests that the issue might be intermittent.





Subject: Request to Raise RFC for 20th July 2024 FRTB Release for MARS GUI Apps

Hi Binoy,

I hope you are doing well.

Could you please raise an RFC for the upcoming FRTB release for the MARS GUI Apps, scheduled for July 20th, 2024?

Thank you for your attention to this matter.

Best Regards
Niraj



Subject: Urgent: Build Failure Notification for RiskPortal Project

Hi Dharmendra,

I hope this message finds you well.

I am writing to inform you about a build failure in the RiskPortal project that needs your immediate attention. Below are the details of the failure:

Project: RiskPortal
Branch: feature/RISKPORTAL-270
Commit: c2453fe7d00fa5f63a6aec5f66d4bac2881d9664
Build Number: #7
Date and Time: 2024-06-27T06:42:17.027Z
Error Message: Error: No such object: docker-win.odyssey.apps.csintra.net/com/csg/caas/images/windows/build/vsbuildtools2017-15.9.13-office:4.7.2-dotnet-framework-10.0.14393.3506
It appears that the Docker image required for the build could not be found. This issue has caused the build to fail during the "Publish Artifactory" stage.

Could you please investigate this issue and take the necessary actions to resolve it?

Thank you for your prompt attention to this matter.

Best regards,
Niraj




import React, { useState } from 'react';

const NameChanger = () => {
  // Define an array of names
  const names = [
    'Niraj Kumar',
    'Aarav Singh',
    'Ishaan Sharma',
    'Vivaan Patel',
    'Aditya Verma',
    'Kavya Menon',
    'Aadhya Gupta',
    'Aryan Mehta',
  ];

  // Initialize state with a default name
  const [randomName, setRandomName] = useState('Niraj Kumar');

  // Function to set a random name from the array
  const handleChangeName = () => {
    const randomIndex = Math.floor(Math.random() * names.length);
    setRandomName(names[randomIndex]);
  };

  return (
    <div>
      <h1>{randomName}</h1>
      <button onClick={handleChangeName}>Change Name</button>
    </div>
  );
};

export default NameChanger;





import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
import { HttpClientModule, HttpClient } from '@angular/common/http';
import { AppConfigService } from './app/app-config.service';
import { EnvironmentParserService } from './app/environment-parser.service';
import { Injector, enableProdMode } from '@angular/core';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

// Create an injector for HttpClient and EnvironmentParserService manually
const injector = Injector.create({
  providers: [
    { provide: HttpClientModule, deps: [] },
    { provide: HttpClient, deps: [HttpClientModule] },
    { provide: EnvironmentParserService, deps: [] }
  ]
});

const httpClient = injector.get(HttpClient);
const environmentParserService = injector.get(EnvironmentParserService);
const appConfigService = new AppConfigService(httpClient, environmentParserService);

appConfigService.configLoaded().then(loaded => {
  if (loaded) {
    const fileDetails = appConfigService.getConfigFileDetails();
    console.log('Configuration loaded successfully');
    if (fileDetails) {
      console.log(`File: ${fileDetails.fileName}`);
      console.log(`Size: ${fileDetails.fileSize} bytes`);
      console.log(`Loaded at: ${fileDetails.loadTime.toISOString()}`);
      console.log(`Environment: ${fileDetails.environment}`);
    }
    const currentEnv = appConfigService.getCurrentEnvironment();
    console.log(`Current environment: ${currentEnv}`);
    platformBrowserDynamic([
      { provide: AppConfigService, useValue: appConfigService },
      { provide: EnvironmentParserService, useValue: environmentParserService }
    ])
      .bootstrapModule(AppModule)
      .catch(err => console.error(err));
  } else {
    console.error('Failed to load configuration. Cannot bootstrap the application.');
  }
});









import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { EnvironmentParserService } from './environment-parser.service';

interface ConfigFileDetails {
  loadTime: Date;
  fileName: string;
  fileSize: number;
  environment: string;
}

@Injectable({
  providedIn: 'root'
})
export class AppConfigService {
  private config: any;
  private configLoadedPromise: Promise<boolean>;
  private configFileDetails: ConfigFileDetails | null = null;
  private isLoading: boolean = false;
  private loadQueue: (() => void)[] = [];

  constructor(
    private http: HttpClient,
    private environmentParserService: EnvironmentParserService
  ) {
    this.configLoadedPromise = this.loadAppConfig();
  }

  private async acquireLock(): Promise<void> {
    if (this.isLoading) {
      return new Promise<void>(resolve => this.loadQueue.push(resolve));
    }
    this.isLoading = true;
  }

  private releaseLock(): void {
    this.isLoading = false;
    const nextResolve = this.loadQueue.shift();
    if (nextResolve) {
      this.isLoading = true;
      nextResolve();
    }
  }

  async loadAppConfig(): Promise<boolean> {
    await this.acquireLock();
    try {
      const startTime = new Date();
      this.environmentParserService.parseEnvironment(window.location.hostname);
      const configFile = this.environmentParserService.getConfigFile();
      const environment = this.environmentParserService.getEnvironment();

      const response = await this.http.get(`/assets/${configFile}`, { observe: 'response' }).toPromise();
      
      this.config = response.body;

      const contentLength = response.headers.get('Content-Length');
      
      this.configFileDetails = {
        loadTime: new Date(),
        fileName: configFile,
        fileSize: contentLength ? parseInt(contentLength, 10) : 0,
        environment
      };
      console.log(`Config loaded for ${environment} from ${configFile} in ${this.configFileDetails.loadTime.getTime() - startTime.getTime()} ms`);
      return true;
    } catch (error) {
      console.error('Could not load configuration', error);
      this.configFileDetails = null;
      return false;
    } finally {
      this.releaseLock();
    }
  }

  getConfig(): any {
    return this.config;
  }

  configLoaded(): Promise<boolean> {
    return this.configLoadedPromise;
  }

  getConfigFileDetails(): ConfigFileDetails | null {
    return this.configFileDetails;
  }

  isConfigLoaded(): boolean {
    return this.configFileDetails !== null;
  }

  getCurrentEnvironment(): string {
    return this.configFileDetails ? this.configFileDetails.environment : 'UNKNOWN';
  }
}

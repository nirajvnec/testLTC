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

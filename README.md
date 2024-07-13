main.ts

import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
import { AppConfigService } from './app/app-config.service';
import { EnvironmentParserService } from './app/environment-parser.service';
import { Injector, enableProdMode } from '@angular/core';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

async function bootstrapApplication() {
  // Create an injector for EnvironmentParserService manually
  const injector = Injector.create({
    providers: [
      { provide: EnvironmentParserService, deps: [] }
    ]
  });

  const environmentParserService = injector.get(EnvironmentParserService);
  const appConfigService = await AppConfigService.getInstance(environmentParserService);

  if (appConfigService.isConfigLoaded()) {
    const fileDetails = appConfigService.getConfigFileDetails();
    console.log('Configuration loaded successfully');
    if (fileDetails) {
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
}

bootstrapApplication().catch(err => console.error('Error bootstrapping the application:', err));

config-constants.ts

import devConfig from '../assets/config.dev.json';
import sitConfig from '../assets/config.sit.json';
import uatConfig from '../assets/config.uat.json';
import prodConfig from '../assets/config.prod.json';

export const CONFIG_FILES = {
  DEV: devConfig,
  SIT: sitConfig,
  UAT: uatConfig,
  PRODUCTION: prodConfig
};


environment-parser.service.ts

import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class EnvironmentParserService {
  private environment: string;

  constructor() {}

  parseEnvironment(hostname: string): void {
    const sitEnvironments = ['FT', 'LT', 'ERC', 'FRTB'];
    const uatEnvironments = ['UAT', 'PTE'];

    if (sitEnvironments.some(env => hostname.toUpperCase().includes(env))) {
      this.environment = 'SIT';
    } else if (uatEnvironments.some(env => hostname.toUpperCase().includes(env))) {
      this.environment = 'UAT';
    } else if (hostname.includes('localhost')) {
      this.environment = 'DEV';
    } else {
      this.environment = 'PRODUCTION';
    }
  }

  getEnvironment(): string {
    return this.environment;
  }
}





app-config.service.ts

import { Injectable } from '@angular/core';
import { EnvironmentParserService } from './environment-parser.service';
import { CONFIG_FILES } from './config.constants';

interface ConfigFileDetails {
  loadTime: Date;
  environment: string;
}

@Injectable({
  providedIn: 'root'
})
export class AppConfigService {
  private static instance: AppConfigService | null = null;
  private static instancePromise: Promise<AppConfigService> | null = null;
  private config: any;
  private configFileDetails: ConfigFileDetails | null = null;

  private constructor(private environmentParserService: EnvironmentParserService) {}

  static async getInstance(environmentParserService: EnvironmentParserService): Promise<AppConfigService> {
    if (AppConfigService.instance === null) {
      if (AppConfigService.instancePromise === null) {
        AppConfigService.instancePromise = (async () => {
          const instance = new AppConfigService(environmentParserService);
          await instance.loadAppConfig();
          AppConfigService.instance = instance;
          return instance;
        })();
      }
      await AppConfigService.instancePromise;
    }
    return AppConfigService.instance!;
  }

  private async loadAppConfig(): Promise<boolean> {
    try {
      const startTime = new Date();
      this.environmentParserService.parseEnvironment(window.location.hostname);
      const environment = this.environmentParserService.getEnvironment();

      this.config = CONFIG_FILES[environment];

      if (!this.config) {
        throw new Error(`No configuration found for environment: ${environment}`);
      }

      this.configFileDetails = {
        loadTime: new Date(),
        environment
      };

      console.log(`Config loaded for ${environment} in ${this.configFileDetails.loadTime.getTime() - startTime.getTime()} ms`);
      return true;
    } catch (error) {
      console.error('Could not load configuration', error);
      this.configFileDetails = null;
      return false;
    }
  }

  getConfig(): any {
    return this.config;
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

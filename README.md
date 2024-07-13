
ng generate class config/app-config --type=config --skip-tests


app.module.ts

import { NgModule, APP_INITIALIZER } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import { AppConfigService } from './app-config.service';
import { EnvironmentParserService } from './environment-parser.service';

export function initializeApp(appConfigService: AppConfigService) {
  return () => appConfigService.loadAppConfig();
}

@NgModule({
  declarations: [
    AppComponent,
    // ... other components
  ],
  imports: [
    BrowserModule,
    HttpClientModule,
    // ... other modules
  ],
  providers: [
    EnvironmentParserService,
    AppConfigService,
    {
      provide: APP_INITIALIZER,
      useFactory: initializeApp,
      deps: [AppConfigService],
      multi: true
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

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
  private config: any;
  private configFileDetails: ConfigFileDetails | null = null;

  constructor(private environmentParserService: EnvironmentParserService) {}

  async loadAppConfig(): Promise<boolean> {
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

import { Injectable } from '@angular/core';
import { EnvironmentParserService } from './environment-parser.service';
import { ENVIRONMENT_CONFIGS } from './config/app-config';

@Injectable({
  providedIn: 'root'
})
export class AppConfigService {
  private config: any;
  private isLoaded = false;

  constructor(private environmentParserService: EnvironmentParserService) {}

  loadAppConfig(): Promise<boolean> {
    if (this.isLoaded) {
      return Promise.resolve(true);
    }

    return new Promise((resolve, reject) => {
      try {
        const hostname = window.location.hostname;
        this.environmentParserService.parseEnvironment(hostname);
        const environment = this.environmentParserService.getEnvironment();
        
        this.config = ENVIRONMENT_CONFIGS[environment];
        if (!this.config) {
          throw new Error(`No configuration found for environment: ${environment}`);
        }
        console.log(`Config loaded for ${environment}`);
        this.isLoaded = true;
        resolve(true);
      } catch (error) {
        console.error('Could not load configuration', error);
        reject(error);
      }
    });
  }

  getConfig(): any {
    if (!this.isLoaded) {
      throw new Error('Configuration not loaded. Call loadAppConfig first.');
    }
    return this.config;
  }
}

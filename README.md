import { Injectable } from '@angular/core';
import { EnvironmentParserService } from './environment-parser.service';
import { ENVIRONMENT_CONFIGS } from './config/app-config';

@Injectable({
  providedIn: 'root'
})
export class AppConfigService {
  private config: any;

  constructor(private environmentParserService: EnvironmentParserService) {}

  loadAppConfig(): boolean {
    try {
      const hostname = window.location.hostname;
      this.environmentParserService.parseEnvironment(hostname);
      const environment = this.environmentParserService.getEnvironment();
      
      this.config = ENVIRONMENT_CONFIGS[environment];
      if (!this.config) {
        throw new Error(`No configuration found for environment: ${environment}`);
      }
      console.log(`Config loaded for ${environment}`);
      return true;
    } catch (error) {
      console.error('Could not load configuration', error);
      return false;
    }
  }

  getConfig(): any {
    return this.config;
  }
}

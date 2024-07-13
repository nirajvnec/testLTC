import { Injectable } from '@angular/core';
import { EnvironmentParserService } from './environment-parser.service';
import { ENVIRONMENT_CONFIGS } from './config/app-config';

@Injectable({
  providedIn: 'root'
})
export class AppConfigService {
  private config: any;

  constructor(private environmentParserService: EnvironmentParserService) {
    this.loadAppConfig();
  }

  private loadAppConfig(): void {
    const hostname = window.location.hostname;
    this.environmentParserService.parseEnvironment(hostname);
    const environment = this.environmentParserService.getEnvironment();
    
    this.config = ENVIRONMENT_CONFIGS[environment];
    if (!this.config) {
      throw new Error(`No configuration found for environment: ${environment}`);
    }
    console.log(`Config loaded for ${environment}`);
  }

  getConfig(): any {
    return this.config;
  }
}

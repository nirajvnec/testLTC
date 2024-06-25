import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class EnvironmentParserService {
  private environment: string;
  private configFile: string;

  constructor() {}

  parseEnvironment(hostname: string): void {
    if (hostname.includes('FT')) {
      this.environment = 'SIT';
      this.configFile = 'config.sit.json';
    } else if (hostname.includes('UAT')) {
      this.environment = 'UAT';
      this.configFile = 'config.uat.json';
    } else if (hostname.includes('localhost')) {
      this.environment = 'DEV';
      this.configFile = 'config.dev.json';
    } else {
      this.environment = 'PRODUCTION';
      this.configFile = 'config.prod.json';
    }
  }

  getEnvironment(): string {
    return this.environment;
  }

  getConfigFile(): string {
    return this.configFile;
  }
}

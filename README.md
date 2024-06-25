import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { EnvironmentParserService } from './environment-parser.service';

interface ConfigFileDetails {
  loadTime: Date;
  fileName: string;
  fileSize: number;
  environment: string;
  envType: string;
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
      const response = await this.http.get('/assets/config.json', { observe: 'response' }).toPromise();
      
      const fullConfig = response.body;
      this.environmentParserService.loadConfig(fullConfig);
      this.config = this.environmentParserService.getConfig();

      const contentLength = response.headers.get('Content-Length');
      const { environment, envType } = this.environmentParserService.getEnvironmentFromUrl(window.location.hostname);
      
      this.configFileDetails = {
        loadTime: new Date(),
        fileName: 'config.json',
        fileSize: contentLength ? parseInt(contentLength, 10) : 0,
        environment,
        envType
      };
      console.log(`Config loaded for ${environment} (${envType}) in ${this.configFileDetails.loadTime.getTime() - startTime.getTime()} ms`);
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

  getCurrentEnvironment(): { environment: string, envType: string } {
    if (this.configFileDetails) {
      return {
        environment: this.configFileDetails.environment,
        envType: this.configFileDetails.envType
      };
    }
    return { environment: 'UNKNOWN', envType: 'UNKNOWN' };
  }
}

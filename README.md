const config = await this.http.get(configFile).toPromise();






import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class EnvironmentParserService {
  private config: any;
  private environment: string;
  private envType: string;

  constructor() {}

  loadConfig(config: any) {
    this.config = config;
    const envFromUrl = this.getEnvironmentFromUrl(window.location.hostname);
    this.environment = envFromUrl.environment;
    this.envType = envFromUrl.envType;
  }

  getEnvironmentFromUrl(hostname: string): { environment: string, envType: string } {
    const envPattern = /(ft\d+|uat\d+)/i;
    const match = hostname.match(envPattern);
    if (match && match[0]) {
      const environment = match[0].toUpperCase();
      const envType = environment.startsWith('FT') ? 'SIT' : 'UAT';
      return { environment, envType };
    }
    return { environment: 'PRODUCTION', envType: 'PRODUCTION' };
  }

  getConfig() {
    if (this.envType && this.config[this.environment]) {
      return this.config[this.environment];
    }
    return this.config['production'];
  }
}







import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { firstValueFrom } from 'rxjs';
import { EnvironmentParserService } from './environment-parser.service';

@Injectable({
  providedIn: 'root'
})
export class AppConfigService {
  private config: any;

  constructor(private http: HttpClient, private envParser: EnvironmentParserService) {}

  async loadConfig() {
    const envType = this.envParser.getEnvironmentFromUrl(window.location.hostname).envType;
    let configFile = '/assets/config.prod.json';

    if (envType === 'SIT') {
      configFile = '/assets/config.sit.json';
    } else if (envType === 'UAT') {
      configFile = '/assets/config.uat.json';
    }

    const config = await firstValueFrom(this.http.get(configFile));
    this.envParser.loadConfig(config);
    this.config = this.envParser.getConfig();
  }

  getConfig() {
    return this.config;
  }
}







import { NgModule, APP_INITIALIZER } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';
import { AppConfigService } from './services/app-config.service';
import { EnvironmentParserService } from './services/environment-parser.service';

export function initializeApp(appConfig: AppConfigService) {
  return () => appConfig.loadConfig();
}

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule
  ],
  providers: [
    AppConfigService,
    EnvironmentParserService,
    {
      provide: APP_INITIALIZER,
      useFactory: initializeApp,
      deps: [AppConfigService],
      multi: true
    },
    {
      provide: 'APP_CONFIG',
      useFactory: (appConfig: AppConfigService) => appConfig.getConfig(),
      deps: [AppConfigService]
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }









import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';
import { environment } from './environments/environment';
import { AppConfigService } from './app/services/app-config.service';
import { HttpClientModule, HttpClient } from '@angular/common/http';
import { EnvironmentParserService } from './app/services/environment-parser.service';
import { firstValueFrom } from 'rxjs';

if (environment.production) {
  enableProdMode();
}

function loadAppConfig() {
  const httpClient = new HttpClient(HttpClientModule);
  const envParser = new EnvironmentParserService();
  const appConfigService = new AppConfigService(httpClient, envParser);

  return appConfigService.loadConfig().then(() => {
    platformBrowserDynamic([{ provide: 'APP_CONFIG', useValue: appConfigService.getConfig() }])
      .bootstrapModule(AppModule)
      .catch(err => console.error(err));
  });
}

loadAppConfig();

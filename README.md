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

import { AxiosError } from 'axios';

export class Utilities {
  public static buildAxiosErrorMessage(error: AxiosError, functionName: string): string {
    const errorParts: string[] = [];

    if (!getMarvelControlsEnv().IS_DEV_ENV) return '';

    errorParts.push(`Function: ${functionName}`);

    if (error.config) {
      const baseURL = error.config.baseURL ?? '';
      const urlPath = error.config.url ?? '';
      const fullUrl = baseURL && !urlPath.startsWith('http') ? `${baseURL}${urlPath}` : urlPath;
      errorParts.push(`URL: ${fullUrl}`);
      errorParts.push(`Method: ${error.config.method?.toUpperCase()}`);
    }

    if (error.response) {
      errorParts.push(`Status Code: ${error.response.status}`);
      errorParts.push(`Status Text: ${error.response.statusText}`);
    } else if (error.code === 'ECONNABORTED') {
      errorParts.push('Status Code: Request Timed Out');
      errorParts.push(`Message: ${error.message}`);
    } else if (error.message) {
      errorParts.push(`Message: ${error.message}`);
    }

    const stackTrace = new Error().stack;
    if (stackTrace) {
      const stackLines = stackTrace.split('\n');
      const callerLine = stackLines[2]?.trim();
      if (callerLine) {
        errorParts.push(`Caller Trace: ${callerLine.replace(/\?\t=\d+:\d+:\d+/, '')}`);
      }
    }

    const finalLog = errorParts.join(' | ');
    console.error(finalLog);
    return finalLog;
  }

  public static handleAxiosError(error: AxiosError, functionName: string, onError?: (msg: string) => void): void {
    const msg = this.buildAxiosErrorMessage(error, functionName);
    if (msg && onError) {
      onError(msg);
    }
  }
}


catch (err) {
  Utilities.handleAxiosError(err, 'onLevelChange', props.onError);
}
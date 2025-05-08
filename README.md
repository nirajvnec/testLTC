public static buildAxiosErrorMessage(error: any, functionName: string): string {
  const errorParts: string[] = [];

  errorParts.push(`Function: ${functionName}`);

  if (error.config) {
    errorParts.push(`URL: ${error.config.url}`);
    errorParts.push(`Method: ${error.config.method?.toUpperCase()}`);
  }

  if (error.response) {
    errorParts.push(`Status Code: ${error.response.status}`);
    errorParts.push(`Status Text: ${error.response.statusText}`);
  } else if (error.code === 'ECONNABORTED') {
    errorParts.push(`Status Code: Request Timed Out`);
    errorParts.push(`Message: ${error.message}`);
  } else if (error.message) {
    errorParts.push(`Message: ${error.message}`);
  }

  const stackTrace = new Error().stack;
  if (stackTrace) {
    const stackLines = stackTrace.split('\n');
    const callerLine = stackLines[2]?.trim();
    const cleanedCallerLine = callerLine?.replace(/\?t=\d+/, ''); // remove ?t=timestamp
    errorParts.push(`Caller Trace: ${cleanedCallerLine}`);
  }

  const finalLog = errorParts.join(' | ');
  console.error(finalLog);
  return finalLog;
}
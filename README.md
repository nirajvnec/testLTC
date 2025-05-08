if (error.config) {
  const baseURL = error.config.baseURL || '';
  const fullURL = `${baseURL}${error.config.url}`;
  errorParts.push(`URL: ${fullURL}`);
  errorParts.push(`Method: ${error.config.method?.toUpperCase()}`);
}
try {
  // your API call or logic here
} catch (error) {
  const stackLines = new Error().stack?.split('\n');
  const callerInfo = stackLines?.[1]?.trim(); // second line usually shows the origin
  const fullError = `Error: ${(error as Error).message} | Location: ${callerInfo}`;

  props.onError?.(fullError);
}
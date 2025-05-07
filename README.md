try {
  // some logic here that might fail
} catch (err) {
  const errorMsg = Utilities.buildErrorMessage(err, 'functionName'); // Your utility
  props.onError?.(errorMsg); // Trigger the error handler prop
}
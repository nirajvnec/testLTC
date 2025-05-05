function showValidationMessages(
  errors: string[],
  setValidationBody: (errors: string[]) => void,
  setShowValidationOverlay: (flag: boolean) => void
): boolean {
  if (errors.length > 0) {
    setValidationBody(errors);
    setShowValidationOverlay(true);
    return true;
  }
  return false;
}
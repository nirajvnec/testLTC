function formatMMDDYYYYorYYYYMMDD(dateStr: string | null | undefined): string {
  if (!dateStr) {
    throw new Error('Date string is null or undefined');
  }

  // Input formats supported: 'MM-DD-YYYY' or 'YYYY-MM-DD'
  // Output format: 'DD-MMM-YYYY' (example: 27-Sep-2024)
  const date = moment(dateStr, ['MM-DD-YYYY', 'YYYY-MM-DD'], true); // strict parsing

  if (!date.isValid()) {
    throw new Error(`Invalid date format: "${dateStr}". Expected formats: MM-DD-YYYY or YYYY-MM-DD.`);
  }

  return date.format('DD-MMM-YYYY');
}

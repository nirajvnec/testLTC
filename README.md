public static parseAndFormatIsoDateTime(dateTimeStr: string): string {
  try {
    if (!moment(dateTimeStr, moment.ISO_8601, true).isValid()) {
      console.log(`Invalid format: Datetime '${dateTimeStr}' is not ISO 8601`);
      return 'Invalid Datetime';
    }

    return moment.utc(dateTimeStr).format('DD-MMM-YYYY HH:mm:ss');
  } catch (error) {
    console.error((error as Error).message);
    return 'Invalid Datetime';
  }
}

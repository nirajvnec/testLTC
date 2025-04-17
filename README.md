public static formatISODateTimeSafely(dateTimeStr: string): string {
  console.log('dateTimeStr:', dateTimeStr);

  try {
    if (!moment(dateTimeStr, moment.ISO_8601, true).isValid()) {
      console.log(
        `Contract broken: Datetime '${dateTimeStr}' is not in the correct format (YYYY-MM-DDTHH:mm:ss.SSSZ)`
      );
      return 'Invalid Datetime';
    }

    return moment.utc(dateTimeStr).format('DD-MMM-YYYY HH:mm:ss');
  } catch (error) {
    console.error((error as Error).message);
    return 'Invalid Datetime';
  }
}

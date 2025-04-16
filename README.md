public static readonly DATE_FORMAT_DD_MMM_YYYY = 'DD-MMM-YYYY';
public static readonly TIME_FORMAT_24H = 'HH:mm:ss';

const DATE_FORMAT_YYYY_MMM_DD_HH_MM_SS = 'YYYY-MMM-DD HH:MM:SS';

public static formatDateTimeAsString(input: string): string {
  const parsed = moment(input, DateTimeFormatter.DATE_FORMAT_YYYY_MMM_DD_HH_MM_SS, true);

  if (!parsed.isValid()) {
    // Use the semantic constant name for the expected format
    throw new Error(`Invalid date format. Expected format: ${DATE_FORMAT_YYYY_MMM_DD_HH_MM_SS}. Contract broken due to invalid input.`);
  }

  return parsed.format(
    `${DateTimeFormatter.DATE_FORMAT_DD_MMM_YYYY} ${DateTimeFormatter.TIME_FORMAT_24H}`
  );
}

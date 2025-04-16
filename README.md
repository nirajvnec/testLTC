public static readonly DATE_FORMAT_DD_MMM_YYYY = 'DD-MMM-YYYY';
public static readonly TIME_FORMAT_24H = 'HH:mm:ss';

static formatDateTimeAsString(input: string): string {
    const parsed = moment(input, DateTimeFormatter.DATE_FORMAT_YYYY_MMM_DD_HH_MM_SS, true);

    if (!parsed.isValid()) {
      throw new Error('Invalid date format');
    }

    return parsed.format(
      `${DateTimeFormatter.DATE_FORMAT_DD_MMM_YYYY} ${DateTimeFormatter.TIME_FORMAT_24H}`
    );
  }

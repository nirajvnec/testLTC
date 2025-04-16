public static readonly DATE_FORMAT_YYYY_MMM_DD_HH_MM_SS = 'YYYY-MMM-DD HH:mm:ss';

  static formatDateTime(input: string): { date: string; time: string } {
    const parsed = moment(input, DateTimeFormatter.DATE_FORMAT_YYYY_MMM_DD_HH_MM_SS, true);

    if (!parsed.isValid()) {
      throw new Error('Invalid date format');
    }

    return {
      date: parsed.format('DD-MMM-YYYY'),
      time: parsed.format('HH:mm:ss'),
    };
  }

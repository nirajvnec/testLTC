import moment from 'moment';

export class DateUtils {
  // Format constants are clearly named by structure only
  private static readonly FORMAT_DD_MM_YYYY = 'DD/MM/YYYY';
  private static readonly FORMAT_DD_MMM_YYYY = 'DD-MMM-YYYY';

  static formatDateToShortMonth(inputDate: string): string {
    const parsed = moment(inputDate, DateUtils.FORMAT_DD_MM_YYYY, true);
    if (!parsed.isValid()) {
      throw new Error(`Invalid date format. Expected ${DateUtils.FORMAT_DD_MM_YYYY}.`);
    }
    return parsed.format(DateUtils.FORMAT_DD_MMM_YYYY);
  }
}

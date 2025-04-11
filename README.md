import moment from 'moment';

export class DateUtils {
  private static readonly INPUT_DATE_FORMAT = 'DD/MM/YYYY';
  private static readonly OUTPUT_DATE_FORMAT = 'DD-MMM-YYYY';

  static formatDateToShortMonth(inputDate: string): string {
    const parsed = moment(inputDate, DateUtils.INPUT_DATE_FORMAT, true);
    if (!parsed.isValid()) {
      throw new Error(`Invalid date format. Expected ${DateUtils.INPUT_DATE_FORMAT}.`);
    }
    return parsed.format(DateUtils.OUTPUT_DATE_FORMAT);
  }
}

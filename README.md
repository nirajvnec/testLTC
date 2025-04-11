import moment from 'moment';

export class DateUtils {
  static formatDateToShortMonth(inputDate: string): string {
    const parsed = moment(inputDate, 'DD/MM/YYYY', true);
    if (!parsed.isValid()) {
      throw new Error('Invalid date format. Expected DD/MM/YYYY.');
    }
    return parsed.format('DD-MMM-YYYY');
  }
}

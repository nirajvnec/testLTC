import moment from 'moment';

export class Utilities {
  // Semantic format constants
  private static readonly FORMAT_MM_DD_YYYY = 'MM-DD-YYYY';
  private static readonly FORMAT_YYYY_MM_DD = 'YYYY-MM-DD';
  private static readonly FORMAT_DD_MMM_YYYY = 'DD-MMM-YYYY';

  public static formatMMDDYYYYorYYYYMMDD(dateStr: string | null | undefined): string {
    if (!dateStr) {
      throw new Error('Date string is null or undefined');
    }

    // Supported input formats
    const inputFormats = [
      Utilities.FORMAT_MM_DD_YYYY,
      Utilities.FORMAT_YYYY_MM_DD
    ];

    const date = moment(dateStr, inputFormats, true); // strict parsing

    if (!date.isValid()) {
      throw new Error(`Invalid date format: "${dateStr}". Expected formats: ${inputFormats.join(' or ')}`);
    }

    return date.format(Utilities.FORMAT_DD_MMM_YYYY);
  }

  public static formatDateSafely(dateStr: string): string {
    if (!moment(dateStr, Utilities.FORMAT_YYYY_MM_DD, true).isValid()) {
      throw new Error(`Contract broken: '${dateStr}' is not in ${Utilities.FORMAT_YYYY_MM_DD} format`);
    }

    return moment(dateStr).format(Utilities.FORMAT_DD_MMM_YYYY);
  }
}

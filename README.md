import moment from "moment";

export class DateTimeUtils {
  /**
   * Formats an ISO datetime string safely.
   * Expected input format: YYYY-MM-DDTHH:mm:ss.SSSZ (ISO 8601).
   * Output format: DD-MMM-YYYY HH:mm:ss (UTC preserved).
   */
  public static formatISODateTimeSafely(dateTimeStr: string): string {
    try {
      if (!moment(dateTimeStr, moment.ISO_8601, true).isValid()) {
        throw new Error(
          `Contract broken: Datetime '${dateTimeStr}' is not in the correct format (YYYY-MM-DDTHH:mm:ss.SSSZ)`
        );
      }
      return moment.utc(dateTimeStr).format("DD-MMM-YYYY HH:mm:ss");
    } catch (error) {
      console.error(error.message);
      return "Invalid Datetime";
    }
  }

  /**
   * Formats a date string safely.
   * Expected input format: YYYY-MM-DD.
   * Output format: DD-MMM-YYYY.
   */
  public static formatDateSafely(dateStr: string): string {
    try {
      if (!moment(dateStr, "YYYY-MM-DD", true).isValid()) {
        throw new Error(`Contract broken: Date '${dateStr}' is not in YYYY-MM-DD format`);
      }
      return moment(dateStr).format("DD-MMM-YYYY");
    } catch (error) {
      console.error(error.message);
      return "Invalid Date";
    }
  }
}


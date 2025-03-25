import moment from "moment";

export class DateTimeUtils {
  /**
   * Safely formats a timestamp.
   * - If the timestamp is already in 'DD-MMM-YYYY h:mm:ss A' format, return it as is.
   * - Otherwise, convert it from 'M/D/YYYY h:mm:ss A' to 'DD-MMM-YYYY h:mm:ss A'.
   * - Returns an empty string if the input is invalid or empty.
   */
  public static formatMDYDateTimeWithAMPM(timestamp?: string): string {
    if (!timestamp) return ""; // Handle empty cases

    // Check if it's already in the target format
    const parsedDate = moment(timestamp, "DD-MMM-YYYY h:mm:ss A", true);
    if (parsedDate.isValid()) {
      return timestamp; // Already formatted correctly
    }

    // Convert if it’s not already formatted
    return moment(timestamp, "M/D/YYYY h:mm:ss A").format("DD-MMM-YYYY h:mm:ss A");
  }
}

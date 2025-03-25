import moment from "moment";

export class DateTimeUtils {
  /**
   * Formats a timestamp from 'M/D/YYYY h:mm:ss A' to 'DD-MMM-YYYY HH:mm:ss' (24-hour format).
   * - If input is already in 'DD-MMM-YYYY HH:mm:ss', return it as is.
   * - Logs an error and returns `"Invalid Datetime"` if the format is incorrect.
   */
  public static formatMDYDateTime24Hour(timestamp?: string): string {
    if (!timestamp) return ""; // Handle empty cases

    // Define expected input and output formats
    const expectedInputFormat = "M/D/YYYY h:mm:ss A"; // US format with AM/PM
    const expectedOutputFormat = "DD-MMM-YYYY HH:mm:ss"; // 24-hour format

    // Check if it's already in the correct format
    if (moment(timestamp, expectedOutputFormat, true).isValid()) {
      return timestamp;
    }

    // Try parsing the input format
    const parsedDate = moment(timestamp, expectedInputFormat, true);
    if (parsedDate.isValid()) {
      return parsedDate.format(expectedOutputFormat);
    }

    // Log error and return a safe fallback
    console.error(
      `Contract broken: Expected format is '${expectedInputFormat}', but received '${timestamp}'`
    );
    return "Invalid Datetime"; // Safe fallback for UI
  }
}

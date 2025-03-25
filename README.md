
import moment from "moment";

export class DateTimeUtils {
  /**
   * Converts 'DD-MM-YYYY' to 'DD-MMM-YYYY' (e.g., '25-03-2025' → '25-Mar-2025').
   * - Ensures strict validation before formatting.
   * - Logs an error and returns `"Invalid Date"` if the format is incorrect.
   */
  public static convertDDMMYYYYToDDMMMYYYY(input?: string): string {
    if (!input) return ""; // Handle empty input

    // Define expected and output formats
    const expectedFormat = "DD-MM-YYYY";
    const outputFormat = "DD-MMM-YYYY";

    // Validate the input format strictly
    const parsedDate = moment(input, expectedFormat, true);
    if (parsedDate.isValid()) {
      return parsedDate.format(outputFormat);
    }

    // Log error and return a safe fallback
    console.error(
      `Contract broken: Expected format is '${expectedFormat}', but received '${input}'`
    );
    return "Invalid Date"; // Safe fallback for UI
  }
}

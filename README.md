import moment from "moment";

export const formatISODateTimeSafely = (dateTimeStr: string): string => {
  try {
    // Strictly validate if the input matches "YYYY-MM-DDTHH:mm:ss.SSSZ"
    if (!moment(dateTimeStr, moment.ISO_8601, true).isValid()) {
      throw new Error(
        `Contract broken: Datetime '${dateTimeStr}' is not in the correct format (YYYY-MM-DDTHH:mm:ss.SSSZ)`
      );
    }

    // Convert to "DD-MMM-YYYY HH:mm:ss" while preserving UTC time
    return moment.utc(dateTimeStr).format("DD-MMM-YYYY HH:mm:ss"); 
  } catch (error) {
    console.error(error.message);
    return "Invalid Datetime"; // Fallback UI in case of an invalid format
  }
};

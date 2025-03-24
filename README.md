import moment from "moment";

export const formatDateSafely = (dateStr: string): string => {
  try {
    if (!moment(dateStr, "YYYY-MM-DD", true).isValid()) {
      throw new Error(`Contract broken: Date '${dateStr}' is not in YYYY-MM-DD format`);
    }
    return moment(dateStr).format("DD-MMM-YYYY"); // Converts to 'DD-MMM-YYYY'
  } catch (error) {
    console.error(error.message);
    return "Invalid Date"; // Fallback for UI
  }
};


valueGetter: (params: any) => formatDateSafely(params.data.fromDate)

 valueGetter: (params: any) => formatDateSafely(params.data.toDate)

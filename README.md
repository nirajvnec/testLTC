// Format dates as "DD-MMM-YYYY" using moment
const formattedFromDate: string = moment(datePickerStart, 'YYYY-MM-DD').format('DD-MMM-YYYY');
const formattedToDate: string = moment(datePickerEnd, 'YYYY-MM-DD').format('DD-MMM-YYYY');

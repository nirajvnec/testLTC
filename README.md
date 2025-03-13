valueSetter: (params) => {
  if (!params.newValue) return false;

  // Try to parse with full date-time format
  let parsedDate = moment(params.newValue, 'M/D/YYYY h:mm:ss A', true);

  // If it's not valid, try parsing with YYYYMMDD format
  if (!parsedDate.isValid()) {
    parsedDate = moment(params.newValue, 'YYYYMMDD', true);
  }

  // If still invalid, reject the change
  if (!parsedDate.isValid()) return false;

  // Store in YYYYMMDD format
  params.data.eventTimeStamp = parsedDate.format('YYYYMMDD');

  return true; // Indicate successful update
},

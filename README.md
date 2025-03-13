valueGetter: (params: { data: { eventTimeStamp?: string } }) => {
  const timestamp = params.data?.eventTimeStamp;
  if (!timestamp) return ''; // Handle empty cases

  // Check if it's already in 'DD-MMM-YYYY h:mm:ss A' format
  const parsedDate = moment(timestamp, 'DD-MMM-YYYY h:mm:ss A', true);
  if (parsedDate.isValid()) {
    return timestamp; // Already formatted correctly
  }

  // Convert if it's not already formatted
  return moment(timestamp, 'M/D/YYYY h:mm:ss A').format('DD-MMM-YYYY h:mm:ss A');
},

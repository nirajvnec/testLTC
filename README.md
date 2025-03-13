response.forEach((i) => {
  let formattedDateTime = i.eventTimeStamp; // Default to existing timestamp

  // Try parsing the timestamp using the expected format
  const parsedDate = moment(i.eventTimeStamp, 'DD-MMM-YYYY h:mm:ss A', true);

  if (!parsedDate.isValid()) {
    // Convert only if the existing timestamp is NOT already in the desired format
    formattedDateTime = moment(i.eventTimeStamp, 'M/D/YYYY h:mm:ss A').format('DD-MMM-YYYY h:mm:ss A');
  }

  console.log(formattedDateTime);
});

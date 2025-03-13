gridRef.current.api.forEachNode((node: RowNode) => {
  if (node.data && node.data.eventTimeStamp) {
    // Get the formatted value using the existing valueGetter logic
    const formattedDate = moment(node.data.eventTimeStamp, ["M/D/YYYY h:mm:ss A", "YYYYMMDD"], true)
      .format("YYYYMMDD");

    console.log("Formatted eventTimeStamp:", formattedDate);
  }
});

const onExportClick = async () => {
  try {
    const formattedFromDate = fromDate.replace(/-/g, "");
    const formattedToDate = toDate.replace(/-/g, "");
    const fileName = `Export_${formattedFromDate}_to_${formattedToDate}.xlsx`;

    // Fetch all data from the server
    const response = await fetch("https://your-api.com/export-data");
    const fullData = await response.json(); // Assume the API returns all records

    // Set rowData temporarily for export
    gridRef.current.api.setRowData(fullData);

    // Export the full dataset
    gridRef.current.api.exportDataAsExcel({
      fileName: fileName,
    });

    // Restore paginated data (optional, if needed)
    fetchPaginatedData(); 
  } catch (error) {
    console.error("Error fetching export data:", error);
  }
};

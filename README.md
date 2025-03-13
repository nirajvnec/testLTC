onFirstDataRendered={() => {
  setTimeout(() => {
    console.log("First data rendered:");
    console.log(gridRef.current?.api?.getRowData()); // Check if API is accessible
  }, 500); // Delay to ensure data is loaded
}}

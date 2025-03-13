<AgGridReact
  ref={gridRef}
  columnDefs={columnDefs}
  rowData={rowData}
  onGridReady={(params) => {
    gridRef.current = params.api;
    console.log("Grid is ready!");
  }}
  onFirstDataRendered={() => {
    console.log("First data rendered:");
    console.log(gridRef.current.api.getRowData()); // Logs when data is available
  }}
/>

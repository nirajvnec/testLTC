<AgGridReact
  rowData={reportData}
  columnDefs={columnDefs}
  defaultColDef={defaultColumnDefinition}
  pagination={true}
  paginationPageSize={MIN_GRID_PAGE_SIZE}
  paginationAutoPageSize={false}
  rowSelection="single"
  suppressRowClickSelection={true}
  onPaginationChanged={onPaginationChanged}
  onFirstDataRendered={onFirstDataRendered}
  masterDetail={true} // ✅ Enables Master-Detail
  detailCellRenderer={DetailPanel} // ✅ Uses DetailPanel inside the grid
  getRowHeight={(params) => (params.node.detail ? 200 : 50)} // Adjust height dynamically
  **isRowMaster={(data) => !!data.pvReportName}** // ✅ Ensure grid knows which rows have details
/>

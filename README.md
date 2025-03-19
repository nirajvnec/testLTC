<AgGridReact
  rowData={reportData}
  columnDefs={columnDefs}
  masterDetail={true} // ✅ Enables Master-Detail
  detailCellRenderer={DetailPanel} // ✅ Uses DetailPanel inside the grid
  getRowHeight={(params) => (params.node.detail ? 200 : 50)} // Adjust height dynamically
/>

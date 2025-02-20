
git config --global alias.c "commit -m"

git config --global alias.pushup "push --set-upstream origin"


import * as XLSX from "xlsx";

/**
 * Generates an Excel file with a custom message.
 * @param {string} filename - Name of the Excel file.
 * @param {string} message - Message to display in the Excel file.
 */
export const exportNoRecordsExcel = (filename, message) => {
  const worksheet = XLSX.utils.aoa_to_sheet([[message]]);
  const workbook = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(workbook, worksheet, "Sheet1");
  XLSX.writeFile(workbook, filename);
};





import React, { useState, useRef } from "react";
import { AgGridReact } from "ag-grid-react";
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-alpine.css";
import { exportNoRecordsExcel } from "./exportUtils"; // Import the function

const GridExample = () => {
  const [rowData, setRowData] = useState([]); // No data initially
  const gridRef = useRef();

  const columnDefs = [{ field: "name" }, { field: "age" }];

  const exportToExcel = () => {
    if (rowData.length === 0) {
      exportNoRecordsExcel("No_Records.xlsx", "Zero records were found"); // ✅ Call the utility function
    } else {
      gridRef.current.api.exportDataAsExcel(); // Export AG Grid data normally
    }
  };

  return (
    <div className="ag-theme-alpine" style={{ height: 400, width: "100%" }}>
      <button onClick={exportToExcel}>Export to Excel</button>
      <AgGridReact ref={gridRef} rowData={rowData} columnDefs={columnDefs} />
    </div>
  );
};

export default GridExample;

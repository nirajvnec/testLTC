import { useState, useRef } from "react";
import { AgGridReact } from "ag-grid-react";
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-alpine.css";
import RadioButtonCellRenderer from "./RadioButtonCellRenderer"; // Import custom renderer

const MyComponent = () => {
  const [selectedRow, setSelectedRow] = useState<any | null>(null);
  const gridRef = useRef<AgGridReact>(null);

  const handleRowSelection = (rowData: any) => {
    setSelectedRow(rowData);
  };

  return (
    <div>
      {/* AG Grid */}
      <div className="ag-theme-alpine" style={{ height: 400, width: "100%" }}>
        <AgGridReact
          ref={gridRef}
          columnDefs={[
            {
              headerName: "Select",
              minWidth: 52,
              maxWidth: 52,
              sortable: false,
              suppressRowClickSelection: true, 
              cellRenderer: RadioButtonCellRenderer,
              cellRendererParams: {
                onRowSelected: handleRowSelection, // Pass function to child
              },
            },
            { field: "pvReportName", headerName: "Report Name", sortable: true },
          ]}
          rowSelection="single"
          rowData={[
            { id: 1, pvReportName: "Report A" },
            { id: 2, pvReportName: "Report B" },
          ]}
        />
      </div>

      {/* Button */}
      <Button
        disabled={!selectedRow} // Disable button if no row is selected
        onClick={() => console.log("Selected Row:", selectedRow)}
      >
        Request Email For CoB
      </Button>
    </div>
  );
};

export default MyComponent;

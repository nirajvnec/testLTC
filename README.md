import React, { useEffect, useState } from 'react';
import { AgGridReact } from 'ag-grid-react';
import { ColDef } from 'ag-grid-community';

import { AttributeConfigurationService } from '../services/AttributeConfigurationService'; // or the mock one
import { IColumnsInfo } from '../services/AttributeConfigurationModels';

import 'ag-grid-community/styles/ag-grid.css';
import 'ag-grid-community/styles/ag-theme-alpine.css';

const attributeConfigurationService = new AttributeConfigurationService();

export const ConfigureAttributes: React.FC = () => {
  const [allTableNames, setAllTableNames] = useState<string[]>([]);
  const [selectedTable, setSelectedTable] = useState<string>('');
  const [columnDefs, setColumnDefs] = useState<ColDef[]>([]);
  const [rowData, setRowData] = useState<any[]>([]);

  useEffect(() => {
    const fetchAllTables = async () => {
      const tables = await attributeConfigurationService.getAllConfigTables();
      setAllTableNames(tables.map(t => t.tableName));
    };
    fetchAllTables();
  }, []);

  const handleOnChange = async (value: any) => {
    const tableName = value.selectedItemId;
    console.log('selectedDropdownValue', tableName);

    setSelectedTable(tableName);

    // Fetch columns
    const columns: IColumnsInfo[] = await attributeConfigurationService.getSelectedTableInfo(tableName);
    const colDefs: ColDef[] = columns.map(col => ({
      headerName: col.displayName || col.name,
      field: col.name,
      sortable: true,
      filter: true,
      resizable: true,
    }));
    setColumnDefs(colDefs);

    // Fetch data
    const data = await attributeConfigurationService.getSelectedTableColumnData(tableName);
    setRowData(data);
  };

  return (
    <>
      <div className={styles.pageLayout}>
        <div className={layoutStyles.pageHeader}>
          <h2>Configure Attributes</h2>
        </div>

        <div className={styles.formRow}>
          <Dropdown
            id="ddlDataSource"
            labelBeforeSelect="Configuration Table"
            width="27%"
            onChange={handleOnChange}
            selectedItemId={selectedTable}
            withSearch
          >
            {renderDropdownListItemsByValue(allTableNames)}
          </Dropdown>
        </div>

        <div className="ag-theme-alpine" style={{ height: 400, marginTop: 20 }}>
          <AgGridReact
            columnDefs={columnDefs}
            rowData={rowData}
            defaultColDef={{ flex: 1, minWidth: 120 }}
          />
        </div>
      </div>
    </>
  );
};

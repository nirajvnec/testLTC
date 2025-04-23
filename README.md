const handleOnChange = async (value: any) => {
  const tableName = value.selectedItemId;
  console.log('selectedDropdownValue', tableName);

  setSelectedTable(tableName);

  // Fetch columns metadata
  const columns: IColumnsInfo[] = await attributeConfigurationService.getSelectedTableInfo(tableName);

  // Define radio button column (first column)
  const radioColumn: ColDef = {
    headerName: '',
    field: 'radio',
    width: 50,
    suppressMenu: true,
    suppressSorting: true,
    cellRenderer: (params: any) => (
      <input
        type="radio"
        name="gridRadioGroup"
        checked={params.node.isSelected()}
        onChange={() => {
          params.api.deselectAll();
          params.node.setSelected(true);
        }}
      />
    )
  };

  // Define dynamic metadata columns
  const metaColumns: ColDef[] = columns.map(col => ({
    headerName: col.displayName || col.name,
    field: col.name,
    sortable: true,
    filter: true,
    resizable: true
  }));

  // Set columnDefs with radio column first
  setColumnDefs([radioColumn, ...metaColumns]);

  // Fetch table data
  const data = await attributeConfigurationService.getSelectedTableColumnData(tableName);
  setRowData(data);
};

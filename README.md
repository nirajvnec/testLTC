useEffect(() => {
  const loadColumns = async () => {
    const columnsInfo = await attributeService.getSelectedTableInfo('reference.user');

    // **Sort columns based on displayOrder**
    const sortedColumns = [...columnsInfo].sort((a, b) => (a.displayOrder ?? 9999) - (b.displayOrder ?? 9999));

    const colDefs = sortedColumns.map((col) => ({
      field: col.name,
      headerName: col.displayName || col.name,
      sortable: true,
      filter: true,
      resizable: true,
    }));

    setColumnDefs(colDefs);
  };

  loadColumns();
}, []);
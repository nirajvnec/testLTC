function handleOnApprove(rowData: any) {
  if (rowData) {
    setSelectedRowData(rowData); // optional: if you need this elsewhere
    setshowPopup(true);
  } else {
    console.warn('No row data provided to handleOnApprove');
  }
}
const allRowData: RowData[] = [];
gridRef.current.api.forEachNode((node) => {
  if (node.data) {
    allRowData.push(node.data as RowData); // Type assertion to enforce structure
  }
});
console.log("All Displayed Rows:", allRowData);

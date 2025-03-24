const allRowData = [];
gridRef.current.api.forEachNode((node) => allRowData.push(node.data));
console.log("All Displayed Rows:", allRowData);

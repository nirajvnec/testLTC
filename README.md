const logGridData = () => {
  console.log("gridRef.current:", gridRef.current);
  if (!gridRef.current) {
    console.warn("Grid API is null, skipping data fetch.");
    return;
  }

  const rowDataArray: any[] = [];
  gridRef.current.forEachNode((node: any) => rowDataArray.push(node.data));

  console.log("Grid Data:", rowDataArray);
};

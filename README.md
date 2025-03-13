const logGridData = () => {
  if (!gridRef.current || !gridRef.current.api) {
    console.warn("Grid API is not available yet!");
    return;
  }

  const rowDataArray: any[] = [];
  gridRef.current.api.forEachNode((node: any) => rowDataArray.push(node.data));

  console.log("Grid Data:", rowDataArray);
};

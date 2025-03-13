const onGridReady = useCallback((params: any) => {
  gridRef.current = { api: params.api, columnApi: params.columnApi };
  console.log("Grid is ready!", gridRef.current);
}, []);


const logGridData = () => {
  if (!gridRef.current) {
    console.warn("Grid API is not available yet!");
    return;
  }
  const rowDataArray: any[] = [];
  gridRef.current.forEachNode((node: any) => rowDataArray.push(node.data));
  console.log("Grid Data:", rowDataArray);
};

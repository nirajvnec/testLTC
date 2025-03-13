const onFirstDataRendered = useCallback(() => {
    setTimeout(() => {
      console.log("First data rendered!");
      logGridData();
    }, 500); // Ensure data is fully loaded
  }, []);

  const logGridData = () => {
    if (!gridRef.current) return;

    const rowDataArray: any[] = [];
    gridRef.current.forEachNode((node: any) => rowDataArray.push(node.data));

    console.log("Grid Data:", rowDataArray);
  };

const onFirstDataRendered = useCallback(() => {
    setTimeout(() => {
      console.log("First data rendered!");

      if (!gridRef.current) {
        console.warn("Grid API is not available yet!");
        return;
      }

      logGridData();
    }, 500);
  }, []);

  const logGridData = () => {
    if (!gridRef.current) return;

    const rowDataArray: any[] = [];
    gridRef.current.forEachNode((node: any) => rowDataArray.push(node.data));

    console.log("Grid Data:", rowDataArray);
  };

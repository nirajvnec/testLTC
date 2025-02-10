useEffect(() => {
    const fetchData = async () => {
      if (!fromDate || !toDate) return;
      
      try {
        const response = await fetch(
          `your-api-endpoint?fromDate=${fromDate}&toDate=${toDate}`
        );
        const data = await response.json();
        setGridData(data);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    fetchData();
  }, [fromDate, toDate]); // This useEffect runs when dates change

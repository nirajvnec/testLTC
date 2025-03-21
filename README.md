const [isButtonDisabled, setIsButtonDisabled] = useState(true);


setIsButtonDisabled(selectedRows && selectedRows.length === 0);


const onSelectionChanged = () => {
    const selectedRows = gridRef.current?.api.getSelectedRows();
    setIsButtonDisabled(selectedRows && selectedRows.length === 0);
  };


 disabled={isButtonDisabled} // Disable if no row is selected

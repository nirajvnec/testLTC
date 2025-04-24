function onChangeDelForm(value: any, index: number) {
  const updatedRows = [...rows];

  // Clear the value first (intermediate state)
  updatedRows[index] = {
    ...updatedRows[index],
    drpdwnVal: '' // temp clear
  };
  setState(prev => ({
    ...prev,
    rows: updatedRows
  }));

  // Now reapply selected value
  setTimeout(() => {
    const finalRows = [...updatedRows];
    finalRows[index] = {
      ...finalRows[index],
      drpdwnVal: value.selectedValue
    };
    setState(prev => ({
      ...prev,
      rows: finalRows
    }));
  }, 0);
}

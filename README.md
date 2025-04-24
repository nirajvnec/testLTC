function onChangeDelForm(value: any, index: any) {
  // Instead of directly modifying the array (which might not trigger re-renders)
  // Create a new copy of rows with the updated value
  const newRows = [...rows];
  newRows[index] = {
    ...newRows[index],
    reportDeliveryFormatValue: value,
    reportDeliveryFormatKey: value
  };
  
  // Now update your state with this new array
  // This depends on how your component manages state
  
  // For useState hooks
  // setRows(newRows);
  
  // Or if using class component state
  // this.setState({ rows: newRows });
  
  // Keep your existing state updates for the format values
  setdelvFormat(value);
  setdelvFormatKey(value);
  
  console.log(newRows[index].reportDeliveryFormatValue);
  console.log(delFormat);
  console.log(delFormatKey);
  
  if (selectedMeasure === 'Shared Folder') {
    props.getSubsData(newRows);
  }
}

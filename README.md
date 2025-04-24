function onChangeDelForm(value: any, index: any) {
  // Create a new state object with updated rows
  const updatedRows = [...rows];
  
  // Update the specific row
  updatedRows[index] = {
    ...updatedRows[index],
    reportDeliveryFormatValue: value,
    reportDeliveryFormatKey: value
  };
  
  // Update your state using the appropriate method
  // If using setState in a class component:
  setState({
    ...state,
    rows: updatedRows
  });
  
  // Your existing logic for setting individual state variables
  setdelvFormat(value);
  setdelvFormatKey(value);
  
  if (selectedMeasure === 'Shared Folder') {
    props.getSubsData(updatedRows);
  }
}

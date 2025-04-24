function onChangeDelForm(value: any, index: any) {
  // Create a new array from the current rows
  const updatedRows = [...rows];
  
  // Update only the specified row
  updatedRows[index] = {
    ...updatedRows[index],
    reportDeliveryFormatValue: value,
    reportDeliveryFormatKey: value
  };
  
  // Update state with the new array
  setRows(updatedRows);
  
  // These other state updates might be redundant if you're only using rows for rendering
  setdelvFormat(value);
  setdelvFormatKey(value);
  
  // Debug logging
  console.log(`Updated dropdown ${index} to ${value}`);
  
  // Your existing conditional logic
  if (selectedMeasure === 'Shared Folder') {
    props.getSubsData(updatedRows);
  }
}

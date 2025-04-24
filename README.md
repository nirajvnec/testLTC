function onChangeDelForm(value: any, index: any) {
  // Create a new copy of the row to ensure React detects the change
  const updatedRow = { ...rows[index] };
  updatedRow.reportDeliveryFormatValue = value;
  updatedRow.reportDeliveryFormatKey = value;
  
  // Update the array with the new row object
  rows[index] = updatedRow;
  
  // Force unique values to ensure state updates are detected
  setdelvFormat(`${value}_${Date.now()}`);
  setdelvFormatKey(`${value}_${Date.now()}`);
  
  // If needed, extract the actual value when using these variables:
  // const actualFormat = delvFormat.split('_')[0];
  
  if (selectedMeasure === 'Shared Folder') {
    props.getSubsData([...rows]); // Pass a new array to ensure changes are detected
  }
}

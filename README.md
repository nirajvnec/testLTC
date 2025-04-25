function onChangeDelForm(value: any, index: number) {
  console.log('Inside onChangeDelForm');
  console.log('value.selectedValue', value.selectedValue);
  console.log('value.selectedItemId', value.selectedItemId);

  const updatedRows = [...rows]; // shallow clone array
  updatedRows[index] = {
    ...updatedRows[index], // shallow clone object
    reportDeliveryFormatValue: value.selectedValue,
    reportDeliveryFormatKey: value.selectedItemId
  };

  setState(prev => ({
    ...prev,
    rows: updatedRows
  }));

  setdelvFormatKey(value.selectedItemId);

  if (selectedMeasure === 'Shared Folder') {
    props.getSubsData(updatedRows);
  }
}

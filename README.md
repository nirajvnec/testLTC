function onChangeDelForm(value: any, index: number) {
  console.log('onChangeDelForm');
  console.log('value', value);
  console.log('index', index);

  const updatedRows = [...rows];
  updatedRows[index] = {
    ...updatedRows[index],
    drpdwnVal: value.selectedValue
  };

  setState(prev => ({
    ...prev,
    rows: updatedRows
  }));

  setdelvFormat(value.selectedValue);

  // 🧪 TEMPORARY force re-render
  setTimeout(() => {
    setState(prev => ({ ...prev }));
  }, 0);
}

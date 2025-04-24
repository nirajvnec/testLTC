function onChangeDelForm(value: any, index: number) {
  console.log('onChangeDelForm');
  console.log('value', value);
  console.log('index', index);

  // 1. Create a copy of the rows array
  const updatedRows = [...rows];

  // 2. Update the specific row immutably
  updatedRows[index] = {
    ...updatedRows[index],
    drpdwnVal: value.selectedValue
  };

  // 3. Update the full state object immutably
  setState(prev => ({
    ...prev,
    rows: updatedRows
  }));

  // Optional: still use setdelvFormat if needed for some separate state logic
  setdelvFormat(value.selectedValue);

  console.log(updatedRows[index].drpdwnVal);
}

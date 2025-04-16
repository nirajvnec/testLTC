const columnDefs = [
  {
    field: 'key',
    headerName: 'Field',
    flex: 1,
  },
  {
    field: 'value',
    headerName: 'Value',
    flex: 2,
    valueFormatter: ({ value }) => {
      const formatted = formatDateTimeAsString(value);
      return formatted ?? 'N/A';
    },
  },
];

{
  field: 'reportName',
  headerName: 'Name',
  flex: 2,
  filter: 'agTextColumnFilter',
  sortable: true,
  filterParams: {
    buttons: ['reset', 'apply'],
  },
  tooltipValueGetter: (params) => params.data?.description || '',
  cellStyle: () => textColumnStyle,
}
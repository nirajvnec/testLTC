const [expandedRowData, setExpandedRowData] = useState(null);

const onRowClicked = (params) => {
  setExpandedRowData((prev) => (prev?.id === params.data.id ? null : params.data)); 
};

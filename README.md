const [expandedRowData, setExpandedRowData] = useState(null);

const onRowClicked = (params: { data: ReportInfo }) => {
  setExpandedRowData((prev) => (prev?.pvReportKey === params.data.pvReportKey ? null : params.data));
};

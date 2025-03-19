const DetailPanel = (props: { data: RowData }) => { 
  if (!props.data) return null; // ✅ Check if data exists

  return (
    <div className="detail-panel">
      <h4>Details for {props.data.pvReportName}</h4>
      <p><strong>Description:</strong> {props.data.pvReportDescription}</p>
      <p><strong>Updated By:</strong> {props.data.modifiedBy}</p>
    </div>
  );
};

export default DetailPanel;

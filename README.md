const DetailPanel = ({ rowData }) => {
  if (!rowData) return null;

  return (
    <div className="detail-panel">
      <h4>Details for {rowData.pvReportName}</h4>
      <p><strong>Description:</strong> {rowData.pvReportDescription}</p>
      <p><strong>Updated By:</strong> {rowData.modifiedBy}</p>
    </div>
  );
};

export default DetailPanel;

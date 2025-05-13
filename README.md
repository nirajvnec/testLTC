import React, { useState } from 'react';

// Enum renamed to PBIReportStatus
enum PBIReportStatus {
  Approved = 'approved',
  Declined = 'declined',
}

const PBIReport = () => {
  // State uses the updated enum
  const [status, setStatus] = useState<PBIReportStatus | ''>('');

  return (
    <div>
      <h3>PBI Report</h3>
      <div style={{ display: 'flex', gap: '20px', marginBottom: '10px' }}>
        <label>
          <input
            type="radio"
            name="pbiReportStatus"
            value={PBIReportStatus.Approved}
            checked={status === PBIReportStatus.Approved}
            onChange={() => setStatus(PBIReportStatus.Approved)}
          />
          <span style={{ color: 'green', marginLeft: '5px' }}>Approve PBI Report</span>
        </label>

        <label>
          <input
            type="radio"
            name="pbiReportStatus"
            value={PBIReportStatus.Declined}
            checked={status === PBIReportStatus.Declined}
            onChange={() => setStatus(PBIReportStatus.Declined)}
          />
          <span style={{ color: 'red', marginLeft: '5px' }}>Decline PBI Report</span>
        </label>
      </div>

      <textarea placeholder="Enter your comment" style={{ width: '100%', height: '80px' }} />
      <div style={{ marginTop: '10px' }}>
        <button disabled={!status}>Submit</button>
        <button style={{ marginLeft: '10px' }}>Cancel</button>
      </div>
    </div>
  );
};

export default PBIReport;
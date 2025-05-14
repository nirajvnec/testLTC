import React, { useState } from 'react';

// Enum
enum PBIReportStatus {
  Approved = 'approved',
  Declined = 'declined',
}

const PBIReport = () => {
  const [status, setStatus] = useState<PBIReportStatus | ''>(''); // no default
  const [comment, setComment] = useState(''); // comment state

  const isFormValid = status !== '' && comment.trim().length > 0;

  return (
    <div className={styles.pageLayout}>
      <div className={styles.firstRow}>
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

        <label style={{ marginLeft: '20px' }}>
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

      <div className={styles.secondRow}>
        <textarea
          value={comment}
          onChange={(e) => setComment(e.target.value)}
          placeholder="Enter your comment"
          style={{ width: '100%', height: '80px' }}
        />
      </div>

      <div className={styles.buttonRow}>
        <button onClick={() => {/* submit handler */}} disabled={!isFormValid}>
          Submit
        </button>
        <button style={{ marginLeft: '10px' }}>
          Cancel
        </button>
      </div>
    </div>
  );
};

export default PBIReport;
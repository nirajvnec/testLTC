import React, { useState } from 'react';

const PBIReport = () => {
  const [status, setStatus] = useState('');

  return (
    <div>
      <h3>PBI Report</h3>
      <div style={{ display: 'flex', gap: '20px', marginBottom: '10px' }}>
        <label>
          <input
            type="radio"
            name="pbiStatus"
            value="approved"
            checked={status === 'approved'}
            onChange={() => setStatus('approved')}
          />
          <span style={{ color: 'green', marginLeft: '5px' }}>Approve PBI Report</span>
        </label>

        <label>
          <input
            type="radio"
            name="pbiStatus"
            value="declined"
            checked={status === 'declined'}
            onChange={() => setStatus('declined')}
          />
          <span style={{ color: 'red', marginLeft: '5px' }}>Decline PBI Report</span>
        </label>
      </div>

      <textarea placeholder="Enter your comment" style={{ width: '100%', height: '80px' }} />
      <div style={{ marginTop: '10px' }}>
        <button>Submit</button>
        <button style={{ marginLeft: '10px' }}>Cancel</button>
      </div>
    </div>
  );
};

export default PBIReport;
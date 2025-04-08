import React, { useEffect, useState } from 'react';
import { EmailStatusData } from './EmailStatusData'; // Adjust path if needed

interface PreverificationReportsProps {
  // Define your prop types here
}

export default function ManageReports(props: PreverificationReportsProps) {
  const [emailStatus, setEmailStatus] = useState<EmailStatusData>({
    sent: null,
    pending: null,
    failed: null,
  });

  useEffect(() => {
    const fetchEmailStatus = async () => {
      const data = await getEmailStatusData();
      setEmailStatus(data);
    };

    fetchEmailStatus();
  }, []);

  // Simulated API call – replace with actual service call
  const getEmailStatusData = async (): Promise<EmailStatusData> => {
    return Promise.resolve({
      sent: 9991,
      pending: 9999,
      failed: 9999,
    });
  };

  return (
    <div>
      <div>Sent Emails: {emailStatus.sent}</div>
      <div>Pending Emails: {emailStatus.pending}</div>
      <div>Failed Emails: {emailStatus.failed}</div>
    </div>
  );
}

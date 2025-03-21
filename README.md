

getEmailStatusData(): Promise<EmailStatusData> {
  return Promise.resolve({
    sent: 9999,
    pending: 9999,
    failed: 9999,
  });
}


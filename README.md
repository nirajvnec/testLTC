public async manualPublishPVReport(reportKey: number, cobDate: string): Promise<string> {
  const payload = {
    pvreportkey: reportKey,
    cobdate: cobDate,
  };

  const response = await this._apiClient.post('/PreVerificationReport/ManualPublishPvReport', payload);
  return response.data;
}
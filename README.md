public async saveEmailData(emailRequestModel: EmailRequestModel): Promise<string> {
  const params = {
    pvreportkey: emailRequestModel.pvReportKey,
    cobdate: emailRequestModel.cobDate,
  };

  try {
    const response = await this._apiClient.post(
      '/PreVerificationReport/ManualPublishPvReport',
      null, // No body needed
      { params }
    );

    return response.data;
  } catch (err: any) {
    this._logger?.error('Failed to publish PreVerification Report', err);
    throw err; // rethrow for caller to handle
  }
}




public async saveEmailData(emailRequestModel: EmailRequestModel): Promise<string> {
  const params = {
    pvreportkey: emailRequestModel.pvReportKey,
    cobdate: emailRequestModel.cobDate,
  };

  const response = await this._apiClient.post(
    '/PreVerificationReport/ManualPublishPvReport',
    null, // no request body
    { params }
  );

  return response.data;
}



public async manualPublishPVReport(reportKey: number, cobDate: string): Promise<string> {
  const payload = {
    pvreportkey: reportKey,
    cobdate: cobDate,
  };

  const response = await this._apiClient.post('/PreVerificationReport/ManualPublishPvReport', payload);
  return response.data;
}
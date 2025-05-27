export class PbiReportService implements IPbiReportService {
  constructor(logger: Logger, context: IUserContext) {
    this._getExternalDataSet = this._getExternalDataSet.bind(this);
    this._getReportForFilter = this._getReportForFilter.bind(this);
    this._validateReport = this._validateReport.bind(this);
    this._approveRejectPbiReport = this._approveRejectPbiReport.bind(this);
  }

  async getAllReports(): Promise<Array<PbiReportDataInfo>> {
    try {
      // Get the base URI
      const baseUri = this._apiClient.getUri();
      console.log('Base URI:', baseUri); // Log the base URI for debugging

      // Define the endpoint path for this specific request
      const endpointPath = '/PbiReport'; // Example endpoint for getting reports
      const fullUrl = `${baseUri}${endpointPath}`; // Construct the fully qualified URL

      console.log('Fully Qualified URL:', fullUrl); // Log the fully qualified URL

      // Make the API call using the endpoint path
      const response = await this._apiClient.get<Array<PbiReportDataInfo>>(endpointPath);
      const data = response?.data;

      return data;
    } catch (err: any) {
      throw err;
    }
  }
}
interface PbiReportDataInfo {
  id: number;
  reportName: string;
  description: string;
  status: 'Approved' | 'Pending Approval' | 'Rejected' | 'Draft' | 'Pending';
  approvedBy: string;
  comment: string;
  canSendForApproval: boolean;
  approvalStatus: 'PENDING' | 'APPROVED' | 'REJECTED';
  hasAccess: boolean;
  reportLink: string;
}



getAllPBIReport(): Promise<PbiReportDataInfo[]>;


const mockReportData: PbiReportDataInfo[] = [
  {
    id: 1,
    reportName: 'Report_25March2025_01',
    description: 'Report_25March2025_01 description',
    status: 'Approved',
    approvedBy: 'John Smith',
    comment: 'Last updated by reviewer',
    canSendForApproval: false,
    approvalStatus: 'APPROVED',
    hasAccess: false,
    reportLink: 'https://example.com/report/25march2025_01',
  },
  {
    id: 2,
    reportName: 'Report_10March2025_01',
    description: 'Report_10March2025_01 description',
    status: 'Pending Approval',
    approvedBy: 'John Smith',
    comment: '',
    canSendForApproval: true,
    approvalStatus: 'PENDING',
    hasAccess: true,
    reportLink: 'https://example.com/report/10march2025_01',
  },
  {
    id: 3,
    reportName: 'Report_21March2025_Mail_Word',
    description: 'Report_21March2025_Mail_Word description',
    status: 'Rejected',
    approvedBy: 'John Smith',
    comment: 'Rejected due to missing attachment',
    canSendForApproval: true,
    approvalStatus: 'REJECTED',
    hasAccess: true,
    reportLink: 'https://example.com/report/21march2025_mail_word',
  },
  {
    id: 4,
    reportName: 'Report_15April2025_QA',
    description: 'Report_15April2025_QA description',
    status: 'Draft',
    approvedBy: '',
    comment: '',
    canSendForApproval: true,
    approvalStatus: 'PENDING',
    hasAccess: false,
    reportLink: 'https://example.com/report/15april2025_qa',
  },
  {
    id: 5,
    reportName: 'Report_01April2025_Client',
    description: 'Report_01April2025_Client description',
    status: 'Pending',
    approvedBy: '',
    comment: 'Awaiting client confirmation',
    canSendForApproval: false,
    approvalStatus: 'PENDING',
    hasAccess: true,
    reportLink: 'https://example.com/report/01april2025_client',
  },
];


export class PBIReportServiceMock implements IPBIReportService {
  ...

  private mockReportData: PbiReportDataInfo[] = [
    // full mock data from above here
  ];

  async getAllPBIReport(): Promise<PbiReportDataInfo[]> {
    return Promise.resolve(this.mockReportData);
  }

  ...
}


async getAllPBIReport(): Promise<PbiReportDataInfo[]> {
  try {
    const response = await this._apiClient.get<PbiReportDataInfo[]>('/pbiReports');
    return response.data;
  } catch (error) {
    this._logger.error('Failed to fetch all PBI reports', error);
    throw error;
  }
}
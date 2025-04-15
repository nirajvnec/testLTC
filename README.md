export interface ReportData {
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

export const mockReportData: ReportData[] = [
  {
    id: 1,
    reportName: 'Report_25March2025_01',
    description: 'Report_25March2025_01',
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
    description: 'Report_10March2025_01',
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
    description: 'Report_21March2025_Mail_Word',
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
    description: 'Report_15April2025_QA',
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
    description: 'Report_01April2025_Client',
    status: 'Pending',
    approvedBy: '',
    comment: 'Awaiting client confirmation',
    canSendForApproval: false,
    approvalStatus: 'PENDING',
    hasAccess: true,
    reportLink: 'https://example.com/report/01april2025_client',
  },
];

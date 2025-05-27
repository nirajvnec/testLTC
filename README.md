export enum ReportStatus {
  APPROVED = 'Approved',
  PENDING_APPROVAL = 'Pending_Approval',
  REJECTED = 'Rejected',
  DRAFT = 'Draft',
  PENDING = 'Pending',
}

export interface PbiReportDataInfo {
  reportKey: number;
  reportName: string;
  reportDescription: string;
  reportStatus: ReportStatus; // Use the enum here
  approvedBy: string;
  commentary: string;
  sentForApproval: boolean;
  hasAccess: boolean;
  reportLink: string;
}
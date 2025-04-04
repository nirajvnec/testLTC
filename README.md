export interface ReportData {
  id: number;
  reportName: string;
  description: string;
  status: "Approved" | "Pending Approval" | "Rejected" | "Draft" | "Pending";
  approvedBy?: string; // Optional because not all reports are approved
}


export const mockReportData: ReportData[] = [
  {
    id: 1,
    reportName: "Report_25March2025_01",
    description: "Report_25March2025_01",
    status: "Approved",
    approvedBy: "John Smith",
  },
  {
    id: 2,
    reportName: "Report_10March2025_01",
    description: "Report_10March2025_01",
    status: "Pending Approval",
    approvedBy: "John Smith",
  },
  {
    id: 3,
    reportName: "Report_21March2025_Mail_Word",
    description: "Report_21March2025_Mail_Word",
    status: "Rejected",
    approvedBy: "John Smith",
  },
  {
    id: 4,
    reportName: "Report_21March2025_Mail_PPTX",
    description: "Report_21March2025_Mail_PPTX",
    status: "Draft",
  },
  {
    id: 5,
    reportName: "Report_Test",
    description: "Report_Test",
    status: "Pending",
  },
];

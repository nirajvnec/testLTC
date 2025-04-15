import React from 'react';
import { AgGridReact } from 'ag-grid-react';
import { ColDef } from 'ag-grid-community';
import CommentIcon from '@mui/icons-material/Comment';
import SendIcon from '@mui/icons-material/Send';
import TaskAltIcon from '@mui/icons-material/TaskAlt';
import ArticleIcon from '@mui/icons-material/Article';
import Tooltip from '@mui/material/Tooltip';

const columnDefs: ColDef[] = [
  {
    headerName: 'Comment',
    field: 'comment',
    cellRenderer: CommentCellRenderer,
  },
  {
    headerName: 'Send for Approval',
    field: 'canSendForApproval',
    cellRenderer: SendForApprovalRenderer,
  },
  {
    headerName: 'Approve/Reject',
    field: 'approvalStatus',
    cellRenderer: ApproveRejectRenderer,
  },
  {
    headerName: 'Report',
    field: 'reportLink',
    cellRenderer: ReportLinkRenderer,
  },
];


// 1. Comment Icon Renderer
const CommentCellRenderer = (params: any) => {
  const comment = params.value;
  if (!comment) return null;

  return (
    <Tooltip title={comment}>
      <CommentIcon style={{ cursor: 'pointer', color: '#1976d2' }} />
    </Tooltip>
  );
};

// 2. Send for Approval Renderer
const SendForApprovalRenderer = (params: any) => {
  const canSend = params.value;
  if (!canSend) return null;

  return (
    <Tooltip title="Send for approval">
      <SendIcon style={{ cursor: 'pointer', color: 'orange' }} />
    </Tooltip>
  );
};

// 3. Approve/Reject Renderer
const ApproveRejectRenderer = (params: any) => {
  const { approvalStatus, hasAccess } = params.data;

  if (approvalStatus !== 'PENDING' || !hasAccess) return null;

  return (
    <Tooltip title="Approve / Reject">
      <TaskAltIcon style={{ cursor: 'pointer', color: 'green' }} />
    </Tooltip>
  );
};

// 4. Report Link Renderer
const ReportLinkRenderer = (params: any) => {
  const link = params.value;
  if (!link) return null;

  return (
    <Tooltip title="View report">
      <a href={link} target="_blank" rel="noopener noreferrer">
        <ArticleIcon style={{ color: '#333' }} />
      </a>
    </Tooltip>
  );
};


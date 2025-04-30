const [actionRowData, setActionRowData] = useState<any | null>(null);

function handleOnApprove(rowData: any) { if (rowData) { setActionRowData(rowData); setActionType('Approve'); setshowPopup(true); } }

function handleOnReject(rowData: any) { if (rowData) { setActionRowData(rowData); setActionType('Reject'); setshowPopup(true); } }

type ApproveRejectPopupProps = { showPopup: boolean; setShowPopup: (flag: boolean) => void; actionType: 'Approve' | 'Reject' | null; actionRowData: any; // or a defined type };


<ApproveRejectPopup
  showPopup={showPopup}
  setShowPopup={showPopupDialog}
  actionType={actionType}
  actionRowData={actionRowData}
/>

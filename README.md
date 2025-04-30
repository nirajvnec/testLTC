const [actionType, setActionType] = useState<string | null>(null);

function handleOnApprove(rowData: any) { if (rowData) { setSelectedRowData(rowData); setActionType("approve"); setshowPopup(true); } else { console.warn("No row data for approval."); } }

function handleOnReject(rowData: any) { if (rowData) { setSelectedRowData(rowData); setActionType("reject"); setshowPopup(true); } else { console.warn("No row data for rejection."); } }

type ApproveRejectPopupProps = { showPopup: boolean; setShowPopup: (flag: boolean) => void; actionType: 'approve' | 'reject' | null; };

function ApproveRejectPopup(props: ApproveRejectPopupProps) { const { actionType } = props;

useEffect(() => { console.log("Popup opened for:", actionType); }, [actionType]);

... }


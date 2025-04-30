type ApproveRejectPopupProps = {
  showPopup: boolean;
  setShowPopup: (flag: boolean) => void;
  actionType: 'Approve' | 'Reject' | null;
  selectedRowData: any; // or define a proper type if available
};
function showValidationMessages(
  errors: string[],
  setValidationBody: (errors: string[]) => void,
  setShowValidationOverlay: (flag: boolean) => void
): void {
  if (errors.length > 0) {
    setValidationBody(errors);
    setShowValidationOverlay(true);
  }
}



const handleSave = async () => {
  console.log('Inside handleSave');
  console.log('props.actionRowData?.reportName', props.actionRowData?.reportName);

  const errors: string[] = [];
  console.log('Saving comment:', comment);
  console.log('confirming:', isConfigMode);

  if (!comment.trim()) {
    errors.push('Comment is required.');
  }

  // Use the reusable validation function
  const hasValidationErrors = showValidationMessages(errors, setValidationBody, setShowValidationOverlay);
  if (hasValidationErrors) {
    return;
  }

  console.log(pbiReportService);
  const pbiReportApproveRejectInfo: PbiReportApproveRejectInfo = {
    reportName: props.actionRowData?.reportName,
    comment: comment,
  };

  try {
    const response = await pbiReportService.approveRejectPbiReport(pbiReportApproveRejectInfo);
    console.log(response);
    // Call refreshGrid only on success
    await refreshGrid(); 
  } catch (error) {
    console.error('Error approving/rejecting report:', error);
    errors.push('Failed to approve/reject report.');
    showValidationMessages(errors, setValidationBody, setShowValidationOverlay);
  }
};
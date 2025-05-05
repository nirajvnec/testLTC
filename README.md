const handleSave = async () => {
  const errors: string[] = [];

  if (!comment.trim()) {
    errors.push('Comment is required.');
  }

  if (errors.length > 0) {
    setValidationBody(errors);
    setShowValidationOverlay(true);
    return;
  }

  const pbiReportApproveRejectInfo: PbiReportApproveRejectInfo = {
    reportName: props.actionRowData?.reportName,
    comment: comment,
  };

  try {
    const response = await pbiReportService.approveRejectPbiReport(pbiReportApproveRejectInfo);
    console.log(response);

    // Replace with the actual check depending on your service's return format
    if (response?.success || response?.status === 200) {
      await refreshGrid(); // Call refreshGrid only if response is successful
    } else {
      console.error('Approval/Reject failed:', response);
    }
  } catch (error) {
    console.error('Error calling approveRejectPbiReport:', error);
  }
};
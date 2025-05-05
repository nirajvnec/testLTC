try {
  await pbiReportService.approveRejectPbiReport(pbiReportApproveRejectInfo);
  await refreshGrid();
} catch (error: any) {
  console.error('Error:', error);
  errors.push(error?.message || 'Something went wrong while processing the request.');
  setValidationBody(errors);
  setShowValidationOverlay(true);
}
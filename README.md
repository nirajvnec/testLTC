selectedReport: { [reportName: string]: string } = {}; // Tracks selected IDs for each report name

ngOnInit(): void {
  // Initialize selectedReport for pre-selecting radio buttons
  this.batchResultsData.forEach((data) => {
    if (data.isVersioned) {
      // Only set the first `isVersioned` ID for each reportName
      if (!this.selectedReport[data.reportName]) {
        this.selectedReport[data.reportName] = data.id;
      }
    }
  });
}

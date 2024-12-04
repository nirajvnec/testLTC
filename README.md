batchNameSelected() {
  this.loadSpinnerService.start(); // Start loading spinner
  this.resultStorageService
    .getBatchDataByID(this.batchValue.id, this.toCobDate)
    .pipe(
      finalize(() => {
        this.loadSpinnerService.stop(); // Stop loading spinner
        if (this.batchResultsData && this.batchResultsData.length > 0) {
          this.groupBatchResultsByReportName();
          console.log('Batch results grouped:', this.groupedResultsData);
        } else {
          console.warn('No results to group');
        }
      })
    )
    .subscribe(
      (result: ResultSetDef) => {
        if (result.resultSets !== undefined) {
          this.headers = [
            'Result Set',
            'Version',
            'Create User',
            'Create Date(GMT)',
            'Start Time(GMT)',
            'User',
            'Calc Duration',
            'Result Writing Duration',
            'No.Of Results'
          ];
          this.batchResultsData = result.resultSets;
        }
      },
      (error) => {
        console.error('Error fetching batch data:', error);
      }
    );
}

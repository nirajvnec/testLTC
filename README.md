<div class="mb-3">
  <label for="status" class="form-label">Status:</label>
  <select
    id="status"
    class="form-select"
    [(ngModel)]="selectedStatus"
  >
    <option *ngFor="let status of statuses" [value]="status">
      {{ status }}
    </option>
  </select>
</div>

<!-- Display selected value for debugging -->
<p>Selected Status: {{ selectedStatus }}</p>





<input
            type="radio"
            [id]="data.id"
            [value]="data.id"
            [name]="data.reportName"
            [(ngModel)]="selectedReport[data.reportName]"
          />



console.log('Selected Report Mapping on Init:', this.selectedReport);


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

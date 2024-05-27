Hi Bhava,

I am seeing mostly empty data, and it looks like a data setup will be required. Please check.

Regards,
Niraj


<table class="table table-bordered">
  <thead>
    <tr>
      <th>Name</th>
      <th>JSON Viewer</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let report of getCurrentHierarchiesItems()">
      <td>{{ report.name }}</td>
      <td>
        <table class="table table-sm">
          <tbody>
            <tr *ngFor="let item of getJsonItems(report.jsonString)">
              <td>{{ item.key }}</td>
              <td>{{ item.value }}</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

<div>
  <button class="btn btn-info mr-2" (click)="previousHierarchiesPage()" [disabled]="currentHierarchiesPage === 1">Previous</button>
  <button class="btn btn-success" (click)="nextHierarchiesPage()" [disabled]="currentHierarchiesPage === totalHierarchiesPages()">Next</button>
  <p class="text-success font-italic">Page {{ currentHierarchiesPage }} of {{ totalHierarchiesPages() }}</p>
</div>


getJsonItems(jsonString: string): { key: string, value: any }[] {
  const jsonObject = JSON.parse(jsonString);
  return Object.entries(jsonObject).map(([key, value]) => ({ key, value }));
}


getCurrentHierarchiesItems(): { name: string, jsonString: string }[] {
  const startIndex = (this.currentHierarchiesPage - 1) * this.hierarchiesPerPage;
  const endIndex = startIndex + this.hierarchiesPerPage;
  return this.jsonParsableReports.slice(startIndex, endIndex);
}

previousHierarchiesPage(): void {
  if (this.currentHierarchiesPage > 1) {
    this.currentHierarchiesPage--;
  }
}

nextHierarchiesPage(): void {
  if (this.currentHierarchiesPage < this.totalHierarchiesPages()) {
    this.currentHierarchiesPage++;
  }
}

totalHierarchiesPages(): number {
  return Math.ceil(this.jsonParsableReports.length / this.hierarchiesPerPage);
}

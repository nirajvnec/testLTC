<table class="table table-bordered">
  <thead>
    <tr>
      <th>Name</th>
      <th>JSON Viewer</th>
    </tr>
  </thead>
  <tbody>
    <ng-container *ngFor="let report of getCurrentHierarchiesItems()">
      <tr *ngFor="let node of getNodes(report.jsonString)">
        <td>{{ report.name }}</td>
        <td>
          <pre><code>{{ node | json }}</code></pre>
        </td>
      </tr>
    </ng-container>
  </tbody>
</table>

<div>
  <button class="btn btn-info mr-2" (click)="previousHierarchiesPage()" [disabled]="currentHierarchiesPage === 1">Previous</button>
  <button class="btn btn-success" (click)="nextHierarchiesPage()" [disabled]="currentHierarchiesPage === totalHierarchiesPages()">Next</button>
  <p class="text-success font-italic">Page {{ currentHierarchiesPage }} of {{ totalHierarchiesPages() }}</p>
</div>


getNodes(jsonString: string): any[] {
  const jsonData = JSON.parse(jsonString);
  return jsonData.nodes;
}


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

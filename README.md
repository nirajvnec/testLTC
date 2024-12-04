<div *ngIf="groupedResultsData && Object.keys(groupedResultsData).length > 0" class="table-responsive">
  <table class="table table-bordered table-sm w-auto">
    <thead>
      <tr>
        <th></th>
        <th *ngFor="let header of headers">{{ header }}</th>
      </tr>
    </thead>
    <tbody>
      <!-- Loop through grouped data -->
      <ng-container *ngFor="let groupName of Object.keys(groupedResultsData)">
        <!-- Group Header Row -->
        <tr>
          <td colspan="9" class="bg-light"><strong>{{ groupName }}</strong></td>
        </tr>
        <!-- Rows within the group -->
        <tr *ngFor="let data of groupedResultsData[groupName]">
          <td>
            <input
              type="radio"
              [name]="groupName"
              [value]="data.id"
              [(ngModel)]="selectedResultSet[groupName]"
            />
          </td>
          <td>{{ data.reportName }}</td>
          <td>{{ data.version }}</td>
          <td>{{ data.createUser }}</td>
          <td>{{ data.createDate }}</td>
          <td>{{ data.userName }}</td>
          <td>{{ data.calculationDuration }}</td>
          <td>{{ data.resultWritingDuration }}</td>
          <td>{{ data.numberOfResults }}</td>
        </tr>
      </ng-container>
    </tbody>
  </table>
</div>
<div *ngIf="!groupedResultsData || Object.keys(groupedResultsData).length === 0">
  <p>No data available.</p>
</div>

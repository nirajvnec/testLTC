<div *ngIf="groupedResultsData" class="table-responsive">
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
              [name]="groupName" <!-- Group radio buttons by reportName -->
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
<div class="mb-3 d-grid gap-2 d-md-flex justify-content-md-end">
  <button
    type="submit"
    class="btn btn-primary me-md-2"
    (click)="btn_SaveClick()"
  >
    Save
  </button>
</div>

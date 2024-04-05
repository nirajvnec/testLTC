<div *ngIf="errors.length > 0">
  <button (click)="showErrorPopup()">Show Errors</button>
</div>

<div *ngIf="showErrors" class="error-popup">
  <h3>Errors:</h3>
  <ul>
    <li *ngFor="let error of errors">{{ error }}</li>
  </ul>
  <button (click)="hideErrorPopup()">Close</button>
</div>

<!-- Rest of the template code -->



export class DynamicTableComponent implements OnInit {
  // ...existing properties...
  showErrors = false;
  errors: string[] = [];

  // ...existing methods...

  isColumnHeaderValueMissing(row: string, columnHeaders: string[], reportName: string, metadataKey: string): boolean {
    if (row) {
      const values = row.split(',');
      return values.length !== columnHeaders.length;
    } else {
      const errorMessage = `Error: Row data is undefined for report "${reportName}" and metadata "${metadataKey}".`;
      this.errors.push(errorMessage);
      return true;
    }
  }

  showErrorPopup() {
    this.showErrors = true;
  }

  hideErrorPopup() {
    this.showErrors = false;
  }
}







table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
  font-weight: bold;
}



isColumnHeaderValueMissing(row: string, columnHeaders: string[], reportName: string, metadataKey: string): boolean {
  if (row) {
    const values = row.split(',');
    return values.length !== columnHeaders.length;
  } else {
    console.error(`Error: Row data is undefined for report "${reportName}" and metadata "${metadataKey}".`);
    return true;
  }
}



<tr *ngIf="isColumnHeaderValueMissing(row, getColumnHeaders(reportName, metadataKey), reportName, metadataKey)">
  <td colspan="100%" class="error-message">Missing column header value for one or more columns.</td>
</tr>


jsonData = {
  "MAS Reports": {
    "Mapping_FXVega_Stipulated_Curr": [
      "[SNO, MAS_STIPULATED_CCY_PAIRS]",
      "1,AUDUSD"
    ],
    "Format_IRVega_by_tenor": [
      "[RANK, MRS_Template_Currencies]",
      "1,AUD"
    ],
    "Mapping_Stipulated_Curr": [
      "[RANK, MRS_Template_Currencies]",
      "1,AUD"
    ],
    "Format_IRDelta": [
      "[CURRENCY, INSTRUMENT, SNO]",
      "AUD,Government Debt,l",
      "AUD,Other Debt,2",
      "AUD,Money Market Placements/Borrowings,3",
      "AUD,Forward Rate Agreements,4",
      "AUD,Interest rate futures,5",
      "AUD,Futures on Government Bonds,6",
      "AUD,Interest Rate Swaps,7",
      "AUD,Cross Currency Swaps,8",
      "AUD,FX Swaps and Forwards,9",
      "AUD,Foreign exchange options,10",
      "AUD,Interest rate options,11",
      "AUD,Equity Derivatives,12",
      "AUD,Credit Derivatives,13",
      "AUD,Others,14"
    ]
  },
  "CS Sensitivity Report": {
    "Pegged and Non-Pegged Currencies List": [
      ",AUD"
    ]
  },
  "IB_GTS_KRR": {
    "Country": [
      "SA,EMEA,Middle East,Middle East,SAUDI ARABIA"
    ]
  },
  "MR NCL Credit Cluster Report": {}
};





<div *ngFor="let reportName of reportNames">
  <div *ngIf="isReportNameMissing(reportName)">
    <p class="error-message">Missing report name: "{{ reportName }}".</p>
  </div>

  <div *ngIf="!isReportNameMissing(reportName)">
    <h2>{{ reportName }}</h2>

    <div *ngFor="let metadataKey of jsonData[reportName] | keyvalue">
      <div *ngIf="isMetadataMissing(reportName, metadataKey.key)">
        <p class="error-message">Missing metadata: "{{ metadataKey.key }}" for report name "{{ reportName }}".</p>
      </div>

      <div *ngIf="!isMetadataMissing(reportName, metadataKey.key)">
        <h3>{{ metadataKey.key }}</h3>

        <div *ngIf="isColumnHeaderMissing(reportName, metadataKey.key)">
          <p class="error-message">Missing column header for report name "{{ reportName }}" and metadata "{{ metadataKey.key }}".</p>
        </div>

        <table *ngIf="!isColumnHeaderMissing(reportName, metadataKey.key)">
          <thead>
            <tr>
              <th>Report Name</th>
              <th>Metadata</th>
              <th *ngFor="let header of getColumnHeaders(reportName, metadataKey.key)">{{ header }}</th>
            </tr>
          </thead>
          <tbody>
            <tr *ngFor="let row of getPaginatedRows(reportName, metadataKey.key)">
              <td>{{ reportName }}</td>
              <td>{{ metadataKey.key }}</td>
              <td *ngFor="let value of row.split(','); let i = index">
                <span *ngIf="value.trim()">{{ value.trim() }}</span>
                <span *ngIf="!value.trim()" class="error-message">Missing value for column "{{ getColumnHeaders(reportName, metadataKey.key)[i] }}"</span>
              </td>
            </tr>
            <tr *ngIf="isColumnHeaderValueMissing(row, getColumnHeaders(reportName, metadataKey.key))">
              <td colspan="100%" class="error-message">Missing column header value for one or more columns.</td>
            </tr>
          </tbody>
        </table>

        <div class="pagination" *ngIf="!isColumnHeaderMissing(reportName, metadataKey.key)">
          <button *ngFor="let page of [].constructor(getTotalPages(reportName, metadataKey.key)); let i = index"
                  (click)="goToPage(i + 1)">
            {{ i + 1 }}
          </button>
        </div>
      </div>
    </div>
  </div>
</div>




import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-dynamic-table',
  templateUrl: './dynamic-table.component.html',
  styleUrls: ['./dynamic-table.component.css']
})
export class DynamicTableComponent implements OnInit {
  @Input() jsonData: any;
  reportNames: string[] = [];
  currentPage = 1;
  itemsPerPage = 10;

  ngOnInit() {
    this.reportNames = Object.keys(this.jsonData);
  }

  isReportNameMissing(reportName: string): boolean {
    return !this.jsonData.hasOwnProperty(reportName);
  }

  isMetadataMissing(reportName: string, metadataKey: string): boolean {
    return !this.jsonData[reportName].hasOwnProperty(metadataKey);
  }

  isColumnHeaderMissing(reportName: string, metadataKey: string): boolean {
    return !this.jsonData[reportName][metadataKey][0];
  }

  getColumnHeaders(reportName: string, metadataKey: string): string[] {
    if (this.jsonData[reportName] && this.jsonData[reportName][metadataKey] && this.jsonData[reportName][metadataKey][0]) {
      return this.jsonData[reportName][metadataKey][0].split(',').map((header: string) => header.trim().replace(/[\[\]]/g, ''));
    } else {
      return [];
    }
  }

  getRows(reportName: string, metadataKey: string): any[] {
    if (this.jsonData[reportName] && this.jsonData[reportName][metadataKey]) {
      return this.jsonData[reportName][metadataKey].slice(1);
    } else {
      return [];
    }
  }

  getTotalPages(reportName: string, metadataKey: string): number {
    const totalRows = this.getRows(reportName, metadataKey).length;
    return Math.ceil(totalRows / this.itemsPerPage);
  }

  getPaginatedRows(reportName: string, metadataKey: string): any[] {
    const rows = this.getRows(reportName, metadataKey);
    const startIndex = (this.currentPage - 1) * this.itemsPerPage;
    const endIndex = startIndex + this.itemsPerPage;
    return rows.slice(startIndex, endIndex);
  }

  goToPage(page: number) {
    this.currentPage = page;
  }

  isColumnHeaderValueMissing(row: string, columnHeaders: string[]): boolean {
    const values = row.split(',');
    return values.length !== columnHeaders.length;
  }
}



<app-dynamic-table [jsonData]="myJsonData"></app-dynamic-table>

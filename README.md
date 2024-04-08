ngOnInit() {
  // ...existing code...
  this.checkColumnHeaders();
  this.checkMetadata();
  
  for (const reportName of this.reportNames) {
    for (const metadataKey of Object.keys(this.jsonData[reportName])) {
      if (this.isColumnHeaderPresent(reportName, metadataKey)) {
        this.checkColumnValues(reportName, metadataKey);
      }
    }
  }
}




The JSON object encapsulates data mappings for a series of reports, identified by their names: "MAS Reports", "CS Sensitivity Report", "IB_GTS_KRR", and "MR NCL Credit Cluster Report". Each report contains one or more metadata entries, which define the structure and content of the report's data. The metadata for each report comprises a single column header that describes the data fields, followed by one or more rows of corresponding values.

For the "MAS Reports", the metadata and their respective structures are as follows:

"Mapping_FXVega_Stipulated_Curr" holds the column header "[SNO, MAS_STIPULATED_CCY_PAIRS]", with the value "1,AUDUSD".

"Format_IRVega_by_tenor" contains the column header "[RANK, MRS_Template_Currencies]", with a single row value of "1,AUD".

"Mapping_Stipulated_Curr" also uses the column header "[RANK, MRS_Template_Currencies]", with the same single row value "1,AUD".

"Format_IRDelta" presents a broader set of data under the column header "[CURRENCY, INSTRUMENT, SNO]", encompassing a range of values detailing various financial instruments and their classifications.

The "CS Sensitivity Report" is configured with metadata titled "Pegged and Non-Pegged Currencies List"; however, the column header for this metadata is not specified in the provided structure.

Likewise, the "IB_GTS_KRR" report includes metadata "country", yet it lacks a defined column header.

Lastly, the "MR NCL Credit Cluster Report" appears in the structure without any associated metadata.

It is important to note that for each report, the metadata structure requires a column header, and each header must have at least one corresponding row of values. The current JSON structure needs to be updated to include the missing column headers for the "CS Sensitivity Report" and "IB_GTS_KRR" to adhere to this defined schema.




export class DynamicTableComponent implements OnInit {
  // ...existing properties...
  columnHeaderErrors: string[] = [];
  metadataErrors: string[] = [];
  columnValueErrors: string[] = [];

  // ...existing methods...

  checkColumnHeaders(): void {
    this.columnHeaderErrors = [];
    for (const reportName of this.reportNames) {
      for (const metadataKey of Object.keys(this.jsonData[reportName])) {
        if (!this.isColumnHeaderPresent(reportName, metadataKey)) {
          const errorMessage = `Column header is missing for report "${reportName}" and metadata "${metadataKey}".`;
          this.columnHeaderErrors.push(errorMessage);
        }
      }
    }
  }

  isColumnHeaderPresent(reportName: string, metadataKey: string): boolean {
    return this.jsonData[reportName] && this.jsonData[reportName][metadataKey] && this.jsonData[reportName][metadataKey][0];
  }

  checkMetadata(): void {
    this.metadataErrors = [];
    for (const reportName of this.reportNames) {
      if (Object.keys(this.jsonData[reportName]).length === 0) {
        const errorMessage = `Metadata is missing for report "${reportName}".`;
        this.metadataErrors.push(errorMessage);
      }
    }
  }

  checkColumnValues(reportName: string, metadataKey: string): void {
    const columnHeaders = this.getColumnHeaders(reportName, metadataKey);
    const rows = this.getRows(reportName, metadataKey);

    for (const row of rows) {
      const values = row.split(',');
      const missingColumns = columnHeaders.filter((column, index) => !values[index].trim());
      if (missingColumns.length > 0) {
        const errorMessage = `Missing value(s) for column(s) "${missingColumns.join('", "')}" in report "${reportName}" and metadata "${metadataKey}".`;
        this.columnValueErrors.push(errorMessage);
      }
    }
  }

  // ...existing methods...
}




Optimized User Object and Handle Count for MET: The efficiency of user object and handle counts in MET has been enhanced by updating the breakdown logic. Now, breakdowns are generated based on user demand, avoiding the previous strategy of pre-allocating memory for breakdown objects irrespective of their usage.

Enhanced Responsiveness of Result Output Window for Extensive Reports: By employing the asynchronous await pattern, the responsiveness of the result output window for larger reports has been significantly improved, moving away from the previous method that relied on executing operations on the main thread.

Improved Response Time in Hierarchy Manager: The loading time within the Hierarchy Manager has been dramatically reduced from 114 seconds to 14 seconds, thanks to the application of a parallel programming approach for loading all hierarchies.







isColumnHeaderValueMissing(row: string, columnHeaders: string[], reportName: string, metadataKey: any): boolean {
  if (row) {
    const values = row.split(',');
    if (values.length !== columnHeaders.length) {
      const missingColumns = columnHeaders.filter((column, index) => !values[index].trim());
      const errorMessage = `Error: Missing value(s) for column(s) "${missingColumns.join('", "')}" in report "${reportName}" and metadata "${metadataKey}".`;
      this.errors.push(errorMessage);
      return true;
    }
    return false;
  } else {
    const metadataKeyString = typeof metadataKey === 'string' ? metadataKey : JSON.stringify(metadataKey);
    const errorMessage = `Error: Row data is undefined for report "${reportName}" and metadata "${metadataKeyString}".`;
    this.errors.push(errorMessage);
    return true;
  }
}
<tr *ngFor="let row of getPaginatedRows(reportName, metadataKey.key)">
  <td>{{ reportName }}</td>
  <td>{{ metadataKey.key }}</td>
  <td *ngFor="let value of row.split(','); let i = index">
    <span *ngIf="value.trim()">{{ value.trim() }}</span>
    <span *ngIf="!value.trim()" class="error-message">Missing value for column "{{ getColumnHeaders(reportName, metadataKey.key)[i] }}"</span>
  </td>
</tr>
<tr *ngIf="isColumnHeaderValueMissing(row, getColumnHeaders(reportName, metadataKey.key), reportName, metadataKey.key)">
  <td colspan="100%" class="error-message">Missing column header value for one or more columns.</td>
</tr>




.error-popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 20px;
  border: 1px solid #ccc;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  z-index: 9999;
  width: 400px; /* Set a fixed width */
  height: 300px; /* Set a fixed height */
  overflow-y: auto; /* Add a vertical scrollbar when content overflows */
}



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

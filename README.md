import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css']
})
export class MyComponent implements OnInit {
  // Renamed variable from jsonData to parsedJsonHierarchies
  public parsedJsonHierarchies: any;

  ngOnInit() {
    // Simulating a fetch or data setting where jsonData might initially have the JSON string
    const jsonData = {
      'MAS FRS Hierarchies': {
        'COUNTRY': [
          "[]", // Typically, this should not be a string but an actual empty array
          '{"nodes":[{"title":"Country","key":"_2","isFolder":false, "isLazy":false, "hierarchy":"Country/zz", "sequence":275, "reverse":false}]}'
        ]
      }
    };

    // Ensuring we parse the string if necessary and assign it correctly to parsedJsonHierarchies
    this.parsedJsonHierarchies = {
      'MAS FRS Hierarchies': {
        'COUNTRY': jsonData['MAS FRS Hierarchies']['COUNTRY'].map(item => {
          // Check if item is a string and parse it, otherwise just use it as is
          return typeof item === 'string' ? JSON.parse(item) : item;
        })
      }
    };
  }
}

 
 
 
 
 public parsedJsonHierarchies: any; // Define the new variable


this.parsedJsonHierarchies = JSON.parse(this.jsonData['MAS FRS Hierarchies']['COUNTRY'][1]);



<ng-container *ngIf="parsedJsonHierarchies.nodes">
  <ng-container *ngFor="let country of parsedJsonHierarchies.nodes">
    <tr>
      <td>{{ country.hierarchy }}</td>
      <td>{{ country.name }}</td>
      <td>
        <pre>{{ country | json }}</pre>
      </td>
    </tr>
  </ng-container>
</ng-container>




 this.jsonData['MAS FRS Hierarchies']['COUNTRY'][1] = JSON.parse(this.jsonData['MAS FRS Hierarchies']['COUNTRY'][1]);



<ng-container *ngIf="jsonData['MAS FRS Hierarchies'] && jsonData['MAS FRS Hierarchies']['COUNTRY'][1].nodes">
  <ng-container *ngFor="let country of jsonData['MAS FRS Hierarchies']['COUNTRY'][1].nodes">
    <tr>
      <td>{{ country.hierarchy }}</td>
      <td>{{ country.name }}</td>
      <td>
        <pre>{{ country | json }}</pre>
      </td>
    </tr>
  </ng-container>
</ng-container>




<ng-container *ngFor="let countryObj of jsonData['MAS FRS Hierarchies']['COUNTRY']">
  <!-- Debugging: Show entire countryObj as JSON -->
  <pre>{{ countryObj | json }}</pre>

  <tr *ngFor="let country of countryObj.nodes">
    <td>{{ country.hierarchy }}</td>
    <td>{{ country.name }}</td>
    <!-- Debugging: Show entire country as JSON -->
    <pre>{{ country | json }}</pre>
  </tr>
</ng-container>




function isHierarchyData(data: any): boolean {
    // Check if data is an array and has at least two elements
    if (Array.isArray(data) && data.length > 1) {
        try {
            // Try parsing the second element as JSON
            const obj = JSON.parse(data[1]);
            // Check if the parsed object has a 'nodes' property that is an array
            return 'nodes' in obj && Array.isArray(obj.nodes);
        } catch (error) {
            // If parsing fails or the nodes check fails, log the error and return false
            console.error("Parsing failed or the 'nodes' property is not valid:", error);
            return false;
        }
    }
    return false; // If data is not an array or doesn't have at least two elements
}



function isHierarchyData(data: any): data is HierarchyData[] {
    // Check if data is an array and the first element has a 'nodes' array
    return Array.isArray(data) && data.length > 0 &&
           data[0] != null && typeof data[0] === 'object' &&
           'nodes' in data[0] && Array.isArray(data[0].nodes);
}



function isHierarchyData(data: any): data is HierarchyData[] {
    // Check if data is an array and each element has a 'nodes' array
    return Array.isArray(data) && data.every(item => 
        item != null &&
        typeof item === 'object' && 
        'nodes' in item && Array.isArray(item.nodes) // Simplified check for 'nodes' array
    );
}


function isHierarchyData(data: any): data is HierarchyData[] {
  return data != null && typeof data === 'object' && 'nodes' in data && Array.isArray(data.nodes);
}

function isHierarchyData(data: any): data is HierarchyData[] {
  // Check if data is an array and each item in the array has a 'nodes' property that is also an array
  return Array.isArray(data) && data.every(item => item != null && typeof item === 'object' && 'nodes' in item && Array.isArray(item.nodes));
}



<div class="table-responsive">
  <ng-container *ngFor="let reportName of reportNames">
    <ng-container *ngIf="!isReportNameMissing(reportName)">
      <div *ngFor="let metadatakey of jsonData[reportName] | keyvalue">
        <div *ngIf="!isMetadataMissing(reportName, metadatakey.key)">
          <div *ngIf="!isColumnHeaderMissing(reportName, metadatakey.key)">
            
            <!-- Specific table for 'MAS FRS Hierarchies' -->
            <table class="table table-bordered" *ngIf="reportName === 'MAS FRS Hierarchies'">
              <thead>
                <tr>
                  <th>MARS Hierarchies</th>
                  <th>Country</th>
                  <th>JSON Viewer</th>
                </tr>
              </thead>
              <tbody>
                <ng-container *ngIf="jsonData['MAS FRS Hierarchies'] && isHierarchyData(jsonData['MAS FRS Hierarchies']['COUNTRY'])">
                  <ng-container *ngFor="let countryObj of jsonData['MAS FRS Hierarchies']['COUNTRY']">
                    <tr *ngFor="let country of countryObj.nodes">
                      <td>{{ country.hierarchy }}</td>
                      <td>{{ country.name }}</td>
                      <td>
                        <pre>{{ country | json }}</pre>
                      </td>
                    </tr>
                  </ng-container>
                </ng-container>
              </tbody>
            </table>

            <!-- Other tables not for 'MAS FRS Hierarchies' -->
            <ng-container *ngIf="reportName !== 'MAS FRS Hierarchies'">
              <table class="table table-bordered">
                <thead>
                  <tr>
                    <th>Report Name</th>
                    <th>Metadata</th>
                    <th *ngFor="let header of getColumnHeaders(reportName, metadatakey.key)">
                      {{ header }}
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr *ngFor="let row of getPaginatedRows(reportName, metadatakey.key)">
                    <td>{{ reportName }}</td>
                    <td>{{ metadatakey.key }}</td>
                    <td *ngFor="let value of row.split(','); let i = index">
                      <span *ngIf="value.trim()">{{ value.trim() }}</span>
                    </td>
                  </tr>
                </tbody>
              </table>
              
              <!-- Pagination controls only for other reports -->
              <div class="pagination">
                <button
                  [disabled]="getCurrentPage(reportName, metadatakey.key) === 1"
                  (click)="
                    goToPage(
                      reportName,
                      metadatakey.key,
                      getCurrentPage(reportName, metadatakey.key) - 1
                    )
                  "
                >
                  Previous
                </button>
                <button
                  [disabled]="
                    getCurrentPage(reportName, metadatakey.key) ===
                    getTotalPages(reportName, metadatakey.key)
                  "
                  (click)="
                    goToPage(
                      reportName,
                      metadatakey.key,
                      getCurrentPage(reportName, metadatakey.key) + 1
                    )
                  "
                >
                  Next
              </button>
              </div>
            </ng-container>
            
          </div>
        </div>
      </div>
    </ng-container>
  </ng-container>
</div>




import { Component, Input, OnInit } from '@angular/core';

export interface ReportData {
  [reportName: string]: {
    [metadataKey: string]: string[] | HierarchyData[]; // Allow both strings and hierarchy data arrays
  };
}

export interface HierarchyNode {
  title: string;
  key: string;
  isFolder: boolean;
  isLazy: boolean;
  tooltip: string | null;
  href: string | null;
  icon: string | null;
  addClass: string | null;
  noLink: boolean;
  activate: boolean;
  focus: boolean;
  expand: boolean;
  select: boolean;
  hideCheckbox: boolean;
  unselectable: boolean;
  parent_id: string | null;
  hierarchy: string;
  name: string;
  display_name: string;
  node_id: string;
  parentId?: string;
  __title: string;
  __nodeid: string;
  __parentid: string | null;
  __level: number;
  __path: string;
  __sequence: number;
}

export interface HierarchyData {
  nodes: HierarchyNode[];
  reverse: boolean;
}




@Component({
  selector: 'app-recursive-component',
  templateUrl: './recursive-component.component.html',
  styleUrls: ['./recursive-component.component.css'],
})
export class RecursiveComponentComponent implements OnInit {
  @Input() jsonData: ReportData = {};
  reportNames: string[] = [];
  recordNumber!: number;
  reportTitle!: string;
  itemsPerPage = 1;
  currentPage: { [reportName: string]: { [metadataKey: string]: number } } = {};
  currentReportPage = 0;
  reportsPerPage = 10;
  pageSize = 1;
  metadataPageIndex: {
    [reportName: string]: { [metadataKey: string]: number };
  } = {};
  metadataPageSize: {
    [reportName: string]: { [metadataKey: string]: number };
  } = {};
  actualTotalPages!: number;
  aboutError = false;
  errorMessage!: string;
  columnHeaderSort!: string;
  columnHeaderSortAsc = true;
  reportInit!: {
    this: {
      reportNames: {
        [key: string]: string;
      };
      checkedColumnNames: string[];
      checkBoxEnable: any;
    };
  };
  errorsMessages: string[] = [];
  columnHeaderErrors: string[] = [];
  showErrors = false;
  metadataErrors: string[] = [];
  columnValueErrors: string[] = [];

  ngOnInit() {
    this.reportNames = Object.keys(this.jsonData);
    this.checkColumnHeaders();
    this.checkMetadata();
    for (const reportName of this.reportNames) {
      this.currentPage[reportName] = {};
      this.metadataPageIndex[reportName] = {};
      this.metadataPageSize[reportName] = {};
      for (const metadataKey of Object.keys(this.jsonData[reportName])) {
        this.currentPage[reportName][metadataKey] = 1;
        this.metadataPageIndex[reportName][metadataKey] = 0;
        this.metadataPageSize[reportName][metadataKey] = 10;
        if (this.isColumnHeaderPresent(reportName, metadataKey)) {
          this.checkColumnValues(reportName, metadataKey);
        }
      }
    }
  }

  isHierarchyData(data: any): data is HierarchyData[] {
    return Array.isArray(data) && data.length > 0 && 'nodes' in data[0];
  }
  getCurrentPage(reportName: string, metadataKey: string): number {
    if (!this.currentPage[reportName]) {
      this.currentPage[reportName] = {};
    }
    if (!this.currentPage[reportName][metadataKey]) {
      this.currentPage[reportName][metadataKey] = 1;
    }
    return this.currentPage[reportName][metadataKey];
  }

  showErrorPopup() {
    this.showErrors = true;
  }

  hideErrorPopup() {
    this.showErrors = false;
  }

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
    const data = this.jsonData[reportName][metadataKey];
    if (!data || data.length === 0) return false; // check if data exists and is not empty
  
    const firstItem = data[0];
    // Check if the first item is a string and not empty after trimming
    return typeof firstItem === 'string' && firstItem.trim() !== '';
  }
  

  isColumnHeaderValueMissing(
    row: string,
    columnHeaders: string[],
    reportName: string,
    metadataKey: any
  ): boolean {
    if (row) {
      const values = row.split(',');
      if (values.length !== columnHeaders.length) {
        const missingColumns = columnHeaders.filter(
          (_column, index) => !values[index].trim()
        );
        const errorMessage = `Error: Missing value(s) for column(s) "${missingColumns.join(
          '", "'
        )}" in report "${reportName}" and metadata "${metadataKey}".`;
        this.errorsMessages.push(errorMessage);
        return true;
      }
      return false;
    } else {
      const metadatakeyString =
        typeof metadataKey === 'string'
          ? metadataKey
          : JSON.stringify(metadataKey);
      const errorMessage = `Error: Row data is undefined for report "${reportName}" and metadata "${metadatakeyString}".`;
      this.errorsMessages.push(errorMessage);
      return true;
    }
  }

  getTotalPages(reportName: string, metadataKey: string): number {
    const totalRows = this.getRows(reportName, metadataKey).length;
    return Math.ceil(totalRows / this.itemsPerPage);
  }

  getPaginatedRows(reportName: string, metadataKey: string): string[] {
    const data = this.jsonData[reportName][metadataKey];
    
    // First, make sure we are dealing with an array of strings
    if (!Array.isArray(data) || typeof data[0] !== 'string') {
      // If not, return an empty array because the data is not suitable for pagination
      return [];
    }
    
    const startIndex = (this.getCurrentPage(reportName, metadataKey) - 1) * this.pageSize;
    const endIndex = startIndex + this.pageSize;
  
    // Since we know it's an array of strings, proceed with pagination and processing
    return data.slice(1).slice(startIndex, endIndex).map(row => {
      if (typeof row === 'string') {
        // Split the row into columns based on commas not within quotes
        return row.split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/).map((value, index) => {
          value = value.trim().replace(/^"(.*)"$/, '$1'); // Clean up the values
          if (index === this.getColumnIndex(reportName, metadataKey, 'Responsible')) {
            // Special handling for the 'Responsible' column, reformatting the name
            const nameParts = value.split(',').map(part => part.trim());
            if (nameParts.length === 2) {
              value = `${nameParts[1]} ${nameParts[0]}`;
            }
          }
          return value;
        }).join(', ');
      }
      return ''; // Return an empty string for any non-string data types, although this should not occur
    });
  }
  

  getColumnIndex(reportName: string, metadataKey: string, columnName: string): number {
    const headers = this.getColumnHeaders(reportName, metadataKey);
    return headers.findIndex(header => header.toLowerCase() === columnName.toLowerCase());
  }

  goToPage(reportName: string, metadataKey: string, page: number) {
    if (!this.currentPage[reportName]) {
      this.currentPage[reportName] = {};
    }
    this.currentPage[reportName][metadataKey] = page;
  }

  isReportNameMissing(reportName: string): boolean {
    return !this.jsonData.hasOwnProperty(reportName);
  }

  getColumnHeaders(reportName: string, metadataKey: string): string[] {
    const data = this.jsonData[reportName][metadataKey];
  
    // Ensure the first item is a string before processing it as headers
    if (data.length > 0 && typeof data[0] === 'string') {
      const headers = data[0]
        .replace(/[\[\]]/g, '')  // Remove brackets that might be used to denote arrays
        .split(',')             // Split the string into an array by commas
        .map(header => header.trim());  // Trim whitespace from each header
  
      return headers;
    }
  
    // Return an empty array if the first item isn't a string
    return [];
  }
  
  getRows(reportName: string, metadataKey: string): any[] {
    if (this.jsonData[reportName] && this.jsonData[reportName][metadataKey]) {
      return this.jsonData[reportName][metadataKey].slice(1);
    } else {
      return [];
    }
  }

  isColumnHeaderMissing(reportName: string, metadatakey: string): boolean {
    return !this.jsonData[reportName][metadatakey][0];
  }
  isMetadataMissing(reportName: string, metadataKey: string): boolean {
    return !this.jsonData[reportName].hasOwnProperty(metadataKey);
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
      const missingColumns = columnHeaders.filter(
        (column, index) => !values[index].trim()
      );
      if (missingColumns.length > 0) {
        const errorMessage = `Missing value(s) for column(s) "${missingColumns.join(
          ', '
        )}" in report "${reportName}" and metadata "${metadataKey}".`;
        this.columnValueErrors.push(errorMessage);
      }
    }
  }

  paginatedReports(): string[] {
    const startIndex = this.currentReportPage * this.reportsPerPage;
    const endIndex = startIndex + this.reportsPerPage;
    return this.reportNames.slice(startIndex, endIndex);
  }

  onReportsPerPageChange() {
    this.currentReportPage = 0;
  }

  // onReportPageChange(event: PageEvent) {
  //   this.currentReportPage = event.pageIndex;
  // }

  // onMetadataPageChange(
  //   event: PageEvent,
  //   reportName: string,
  //   metadataKey: string
  // ) {
  //   if (!this.metadataPageIndex[reportName]) {
  //     this.metadataPageIndex[reportName] = {};
  //   }
  //   this.metadataPageIndex[reportName][metadataKey] = event.pageIndex;
  // }

  getTableData(reportName: string, metadataKey: string): any[] {
    const rows = this.getRows(reportName, metadataKey);
    const pageIndex =
      this.metadataPageIndex[reportName] &&
      this.metadataPageIndex[reportName][metadataKey]
        ? this.metadataPageIndex[reportName][metadataKey]
        : 0;
    const startIndex =
      pageIndex * this.metadataPageSize[reportName][metadataKey];
    const endIndex =
      startIndex + this.metadataPageSize[reportName][metadataKey];
    return rows.slice(startIndex, endIndex);
  }
}






















return value.slice(2, -2).replace(/,/g, '').trim();



getCurrentPage(reportName: string, metadataKey: string): number {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  if (!this.currentPage[reportName][metadataKey]) {
    this.currentPage[reportName][metadataKey] = 1;
  }
  return this.currentPage[reportName][metadataKey];
}

getPaginatedRows(reportName: string, metadataKey: string): string[] {
  const data = this.jsonData[reportName][metadataKey];
  const startIndex = (this.getCurrentPage(reportName, metadataKey) - 1) * this.pageSize;
  const endIndex = startIndex + this.pageSize;
  return data.slice(1).slice(startIndex, endIndex).map(row => {
    const values = [];
    let currentValue = '';
    let insideQuotes = false;

    for (let i = 0; i < row.length; i++) {
      const char = row[i];

      if (char === ',' && !insideQuotes) {
        values.push(currentValue.trim());
        currentValue = '';
      } else if (char === '"') {
        if (i > 0 && row[i - 1] === '\\') {
          currentValue = currentValue.slice(0, -1) + char;
        } else {
          insideQuotes = !insideQuotes;
        }
      } else {
        currentValue += char;
      }
    }

    values.push(currentValue.trim());

    const formattedValues = values.map((value, index) => {
      const columnName = this.getColumnName(reportName, metadataKey, index);
      if (columnName === 'Responsible' && value.startsWith('"') && value.endsWith('"')) {
        return value.slice(1, -1);
      }
      return value;
    });

    return formattedValues.join(', ');
  });
}

getColumnIndex(reportName: string, metadataKey: string, columnName: string): number {
  const headers = this.getColumnHeaders(reportName, metadataKey);
  return headers.findIndex(header => header.toLowerCase() === columnName.toLowerCase());
}

getColumnName(reportName: string, metadataKey: string, columnIndex: number): string {
  const headers = this.getColumnHeaders(reportName, metadataKey);
  return headers[columnIndex];
}

goToPage(reportName: string, metadataKey: string, page: number) {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  this.currentPage[reportName][metadataKey] = page;
}








getCurrentPage(reportName: string, metadataKey: string): number {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  if (!this.currentPage[reportName][metadataKey]) {
    this.currentPage[reportName][metadataKey] = 1;
  }
  return this.currentPage[reportName][metadataKey];
}

getPaginatedRows(reportName: string, metadataKey: string): string[] {
  const data = this.jsonData[reportName][metadataKey];
  const startIndex = (this.getCurrentPage(reportName, metadataKey) - 1) * this.pageSize;
  const endIndex = startIndex + this.pageSize;
  return data.slice(1).slice(startIndex, endIndex).map(row => {
    const values = [];
    let currentValue = '';
    let insideQuotes = false;

    for (let i = 0; i < row.length; i++) {
      const char = row[i];

      if (char === ',' && !insideQuotes) {
        values.push(currentValue.trim());
        currentValue = '';
      } else if (char === '"') {
        if (i > 0 && row[i - 1] === '\\') {
          currentValue = currentValue.slice(0, -1) + char;
        } else {
          insideQuotes = !insideQuotes;
        }
      } else {
        currentValue += char;
      }
    }

    values.push(currentValue.trim());

    return values.map((value, index) => {
      const columnName = this.getColumnName(reportName, metadataKey, index);
      if (columnName === 'Responsible') {
        return value.replace(/^"(.*)"$/, '$1');
      }
      return value;
    }).join(', ');
  });
}

getColumnIndex(reportName: string, metadataKey: string, columnName: string): number {
  const headers = this.getColumnHeaders(reportName, metadataKey);
  return headers.findIndex(header => header.toLowerCase() === columnName.toLowerCase());
}

getColumnName(reportName: string, metadataKey: string, columnIndex: number): string {
  const headers = this.getColumnHeaders(reportName, metadataKey);
  return headers[columnIndex];
}

goToPage(reportName: string, metadataKey: string, page: number) {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  this.currentPage[reportName][metadataKey] = page;
}








getCurrentPage(reportName: string, metadataKey: string): number {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  if (!this.currentPage[reportName][metadataKey]) {
    this.currentPage[reportName][metadataKey] = 1;
  }
  return this.currentPage[reportName][metadataKey];
}

getPaginatedRows(reportName: string, metadataKey: string): string[] {
  const data = this.jsonData[reportName][metadataKey];
  const startIndex = (this.getCurrentPage(reportName, metadataKey) - 1) * this.pageSize;
  const endIndex = startIndex + this.pageSize;
  return data.slice(1).slice(startIndex, endIndex).map(row => {
    const values = [];
    let currentValue = '';
    let insideQuotes = false;

    for (let i = 0; i < row.length; i++) {
      const char = row[i];

      if (char === ',' && !insideQuotes) {
        values.push(currentValue.trim().replace(/^"(.*)"$/, '$1'));
        currentValue = '';
      } else if (char === '"' && row[i - 1] === '\\') {
        insideQuotes = !insideQuotes;
        currentValue += char;
      } else {
        currentValue += char;
      }
    }

    values.push(currentValue.trim().replace(/^"(.*)"$/, '$1'));

    return values.join(', ');
  });
}

getColumnIndex(reportName: string, metadataKey: string, columnName: string): number {
  const headers = this.getColumnHeaders(reportName, metadataKey);
  return headers.findIndex(header => header.toLowerCase() === columnName.toLowerCase());
}

goToPage(reportName: string, metadataKey: string, page: number) {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  this.currentPage[reportName][metadataKey] = page;
}










getCurrentPage(reportName: string, metadataKey: string): number {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  if (!this.currentPage[reportName][metadataKey]) {
    this.currentPage[reportName][metadataKey] = 1;
  }
  return this.currentPage[reportName][metadataKey];
}

getPaginatedRows(reportName: string, metadataKey: string): string[] {
  const data = this.jsonData[reportName][metadataKey];
  const startIndex = (this.getCurrentPage(reportName, metadataKey) - 1) * this.pageSize;
  const endIndex = startIndex + this.pageSize;
  return data.slice(1).slice(startIndex, endIndex).map(row => {
    const values = [];
    let currentValue = '';
    let insideQuotes = false;

    for (let i = 0; i < row.length; i++) {
      const char = row[i];

      if (char === ',' && !insideQuotes) {
        values.push(currentValue.trim());
        currentValue = '';
      } else if (char === '"' && row[i - 1] === '\\') {
        insideQuotes = !insideQuotes;
        currentValue += char;
      } else {
        currentValue += char;
      }
    }

    values.push(currentValue.trim());

    return values.map((value, index) => {
      value = value.replace(/^"(.*)"$/, '$1');
      if (index === this.getColumnIndex(reportName, metadataKey, 'Responsible')) {
        const nameParts = value.split(',').map(part => part.trim());
        if (nameParts.length === 2) {
          value = `${nameParts[1]} ${nameParts[0]}`;
        }
      }
      return value;
    }).join(', ');
  });
}

getColumnIndex(reportName: string, metadataKey: string, columnName: string): number {
  const headers = this.getColumnHeaders(reportName, metadataKey);
  return headers.findIndex(header => header.toLowerCase() === columnName.toLowerCase());
}

goToPage(reportName: string, metadataKey: string, page: number) {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  this.currentPage[reportName][metadataKey] = page;
}






<div class="pagination">
  <button
    [disabled]="getCurrentPage(reportName, metadatakey.key) === 1 || (loading[reportName] && loading[reportName][metadatakey.key])"
    (click)="goToPage(reportName, metadatakey.key, getCurrentPage(reportName, metadatakey.key) - 1)"
  >
    <span *ngIf="loading[reportName] && loading[reportName][metadatakey.key]" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
    Previous
  </button>

  <button
    [disabled]="getCurrentPage(reportName, metadatakey.key) === getTotalPages(reportName, metadatakey.key) || (loading[reportName] && loading[reportName][metadatakey.key])"
    (click)="goToPage(reportName, metadatakey.key, getCurrentPage(reportName, metadatakey.key) + 1)"
  >
    <span *ngIf="loading[reportName] && loading[reportName][metadatakey.key]" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
    Next
  </button>
</div>








import { Component } from '@angular/core';
import { from, of } from 'rxjs';
import { catchError, finalize } from 'rxjs/operators';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  loading: { [reportName: string]: { [metadataKey: string]: boolean } } = {};

  goToPage(reportName: string, metadataKey: string, page: number) {
    if (!this.loading[reportName]) {
      this.loading[reportName] = {};
    }
    this.loading[reportName][metadataKey] = true;

    // Simulating an asynchronous operation using RxJS
    const asyncOperation = from(this.performAsyncOperation());

    asyncOperation.pipe(
      catchError((error) => {
        console.error('Error occurred:', error);
        return of(null);
      }),
      finalize(() => {
        this.loading[reportName][metadataKey] = false;
      })
    ).subscribe((result) => {
      console.log('Async operation result:', result);
      // Update the current page after the asynchronous operation is completed
      this.currentPage[reportName][metadataKey] = page;
    });
  }

  private performAsyncOperation(): Promise<any> {
    return new Promise((resolve, reject) => {
      // Simulating an asynchronous operation with a 2-second delay
      setTimeout(() => {
        // Simulating a successful operation
        resolve('Async operation completed successfully');

        // Simulating an error
        // reject('Async operation encountered an error');
      }, 2000);
    });
  }

  // Rest of the component code...
}









import { Component } from '@angular/core';
import { from, of } from 'rxjs';
import { catchError, finalize } from 'rxjs/operators';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  loading = false;

  goToPage(reportName: string, metadataKey: string, page: number) {
    this.loading = true;

    // Simulating an asynchronous operation using RxJS
    const asyncOperation = from(this.performAsyncOperation());

    asyncOperation.pipe(
      catchError((error) => {
        console.error('Error occurred:', error);
        return of(null);
      }),
      finalize(() => {
        this.loading = false;
      })
    ).subscribe((result) => {
      console.log('Async operation result:', result);
      // Update the current page after the asynchronous operation is completed
      this.currentPage[reportName][metadataKey] = page;
    });
  }

  private performAsyncOperation(): Promise<any> {
    return new Promise((resolve, reject) => {
      // Simulating an asynchronous operation with a 2-second delay
      setTimeout(() => {
        // Simulating a successful operation
        resolve('Async operation completed successfully');

        // Simulating an error
        // reject('Async operation encountered an error');
      }, 2000);
    });
  }

  // Rest of the component code...
}





<div class="pagination">
  <button
    [disabled]="getCurrentPage(reportName, metadatakey.key) === 1 || loading"
    (click)="goToPage(reportName, metadatakey.key, getCurrentPage(reportName, metadatakey.key) - 1)"
  >
    <span *ngIf="loading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
    Previous
  </button>

  <button
    [disabled]="getCurrentPage(reportName, metadatakey.key) === getTotalPages(reportName, metadatakey.key) || loading"
    (click)="goToPage(reportName, metadatakey.key, getCurrentPage(reportName, metadatakey.key) + 1)"
  >
    <span *ngIf="loading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
    Next
  </button>
</div>













import { Component } from '@angular/core';
import { from, of } from 'rxjs';
import { catchError, finalize } from 'rxjs/operators';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  loading = false;

  onNextClick() {
    this.loading = true;

    // Simulating an asynchronous operation using RxJS
    const asyncOperation = from(this.performAsyncOperation());

    asyncOperation.pipe(
      catchError((error) => {
        console.error('Error occurred:', error);
        return of(null);
      }),
      finalize(() => {
        this.loading = false;
      })
    ).subscribe((result) => {
      console.log('Async operation result:', result);
      // Handle the result of the asynchronous operation if needed
    });
  }

  private performAsyncOperation(): Promise<any> {
    return new Promise((resolve, reject) => {
      // Simulating an asynchronous operation with a 2-second delay
      setTimeout(() => {
        // Simulating a successful operation
        resolve('Async operation completed successfully');

        // Simulating an error
        // reject('Async operation encountered an error');
      }, 2000);
    });
  }
}





<button (click)="onNextClick()">
  <span *ngIf="loading" class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
  Next
</button>





app.component.ts
// prettier-ignore
import { Component } from '@angular/core';
import { RecursiveComponentComponent } from './app/components/recursive-component/recursive-component.component';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'angular-tour-of-heroes';

  jsonData = {
    'Proxy Time Series': {
      'ISIN Data': [
        '[ISIN, CURRENCY, KEY]',
        'AU000000AACA9, AUD, AU000000AACA9AUD',
        'AU000000ABC7, AUD, AU000000ABC7AUD',
        'AU000000ABP9, AUD, AU000000ABP9AUD',
      ],
    },
    'IRC Report Metadata': {
      'Exclusions_EMG SP': [
        '[Rules, Position Key, Category, Reason, Responsible, Date reviewed, Review required]',
        "Check ABS DaR(%) > 20% ABS Notional,568850195_SAUDI1USNACBD_LTD RECOURSE SPV OPTION, Excluded, This rule does not apply to Gap Option trades. Source IDR ladders are being used for IRC and the Notional/MV does not impact the calculation of DAR.,Kiran Paramane,,",
      ],
      'Exclusions_APAC Credit': [
        '[Rules, Position Key, Category, Reason, Responsible, Date reviewed, Review required]',
        "Check Migration Delta is not 0 or NULL if Mig Type <> 'Non-Linear', SAUDI1USNACBD_BWQ_RISK ANNUITY_20211220_USD, Excluded, MV is very small ,\"Jain, Saurabh\",,FALSE"
      ],
    },
  };
}

app.component.html
<h1>This is app component</h1>
<app-recursive-component [jsonData]="jsonData"></app-recursive-component>

recursive-component.component.ts
import { Component, Input, OnInit } from '@angular/core';

interface ReportData {
  [reportName: string]: {
    [metadataKey: string]: string[];
  };
}

@Component({
  selector: 'app-recursive-component',
  templateUrl: './recursive-component.component.html',
  styleUrls: ['./recursive-component.component.css'],
})
export class RecursiveComponentComponent implements OnInit {
  @Input() jsonData: ReportData = {};
  reportNames: string[] = [];
  recordNumber!: number;
  reportTitle!: string;
  itemsPerPage = 1;
  currentPage: { [reportName: string]: { [metadataKey: string]: number } } = {};
  currentReportPage = 0;
  reportsPerPage = 10;
  pageSize = 1;
  metadataPageIndex: {
    [reportName: string]: { [metadataKey: string]: number };
  } = {};
  metadataPageSize: {
    [reportName: string]: { [metadataKey: string]: number };
  } = {};
  actualTotalPages!: number;
  aboutError = false;
  errorMessage!: string;
  columnHeaderSort!: string;
  columnHeaderSortAsc = true;
  reportInit!: {
    this: {
      reportNames: {
        [key: string]: string;
      };
      checkedColumnNames: string[];
      checkBoxEnable: any;
    };
  };
  errorsMessages: string[] = [];
  columnHeaderErrors: string[] = [];
  showErrors = false;
  metadataErrors: string[] = [];
  columnValueErrors: string[] = [];

  ngOnInit() {
    this.reportNames = Object.keys(this.jsonData);
    this.checkColumnHeaders();
    this.checkMetadata();
    for (const reportName of this.reportNames) {
      this.currentPage[reportName] = {};
      this.metadataPageIndex[reportName] = {};
      this.metadataPageSize[reportName] = {};
      for (const metadataKey of Object.keys(this.jsonData[reportName])) {
        this.currentPage[reportName][metadataKey] = 1;
        this.metadataPageIndex[reportName][metadataKey] = 0;
        this.metadataPageSize[reportName][metadataKey] = 10;
        if (this.isColumnHeaderPresent(reportName, metadataKey)) {
          this.checkColumnValues(reportName, metadataKey);
        }
      }
    }
  }

  getCurrentPage(reportName: string, metadataKey: string): number {
    if (!this.currentPage[reportName]) {
      this.currentPage[reportName] = {};
    }
    if (!this.currentPage[reportName][metadataKey]) {
      this.currentPage[reportName][metadataKey] = 1;
    }
    return this.currentPage[reportName][metadataKey];
  }

  showErrorPopup() {
    this.showErrors = true;
  }

  hideErrorPopup() {
    this.showErrors = false;
  }

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
    return (
      this.jsonData[reportName] &&
      this.jsonData[reportName][metadataKey] &&
      this.jsonData[reportName][metadataKey][0] !== undefined &&
      this.jsonData[reportName][metadataKey][0].trim() !== ''
    );
  }

  isColumnHeaderValueMissing(
    row: string,
    columnHeaders: string[],
    reportName: string,
    metadataKey: any
  ): boolean {
    if (row) {
      const values = row.split(',');
      if (values.length !== columnHeaders.length) {
        const missingColumns = columnHeaders.filter(
          (_column, index) => !values[index].trim()
        );
        const errorMessage = `Error: Missing value(s) for column(s) "${missingColumns.join(
          '", "'
        )}" in report "${reportName}" and metadata "${metadataKey}".`;
        this.errorsMessages.push(errorMessage);
        return true;
      }
      return false;
    } else {
      const metadatakeyString =
        typeof metadataKey === 'string'
          ? metadataKey
          : JSON.stringify(metadataKey);
      const errorMessage = `Error: Row data is undefined for report "${reportName}" and metadata "${metadatakeyString}".`;
      this.errorsMessages.push(errorMessage);
      return true;
    }
  }

  getTotalPages(reportName: string, metadataKey: string): number {
    const totalRows = this.getRows(reportName, metadataKey).length;
    return Math.ceil(totalRows / this.itemsPerPage);
  }

  getPaginatedRows(reportName: string, metadataKey: string): string[] {
    const data = this.jsonData[reportName][metadataKey];
    const startIndex = (this.getCurrentPage(reportName, metadataKey) - 1) * this.pageSize;
    const endIndex = startIndex + this.pageSize;
    return data.slice(1).slice(startIndex, endIndex).map(row => {
      return row.split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/).map((value, index) => {
        value = value.trim().replace(/^"(.*)"$/, '$1');
        if (index === this.getColumnIndex(reportName, metadataKey, 'Responsible')) {
          const nameParts = value.split(',').map(part => part.trim());
          if (nameParts.length === 2) {
            value = `${nameParts[1]} ${nameParts[0]}`;
          }
        }
        return value;
      }).join(', ');
    });
  }

  getColumnIndex(reportName: string, metadataKey: string, columnName: string): number {
    const headers = this.getColumnHeaders(reportName, metadataKey);
    return headers.findIndex(header => header.toLowerCase() === columnName.toLowerCase());
  }

  goToPage(reportName: string, metadataKey: string, page: number) {
    if (!this.currentPage[reportName]) {
      this.currentPage[reportName] = {};
    }
    this.currentPage[reportName][metadataKey] = page;
  }

  isReportNameMissing(reportName: string): boolean {
    return !this.jsonData.hasOwnProperty(reportName);
  }

  getColumnHeaders(reportName: string, metadataKey: string): string[] {
    const data = this.jsonData[reportName][metadataKey];
    if (data.length > 0) {
      const headers = data[0]
        .replace(/[\[\]]/g, '')
        .split(',')
        .map((header) => header.trim());
      return headers;
    }
    return [];
  }
  getRows(reportName: string, metadataKey: string): any[] {
    if (this.jsonData[reportName] && this.jsonData[reportName][metadataKey]) {
      return this.jsonData[reportName][metadataKey].slice(1);
    } else {
      return [];
    }
  }

  isColumnHeaderMissing(reportName: string, metadatakey: string): boolean {
    return !this.jsonData[reportName][metadatakey][0];
  }
  isMetadataMissing(reportName: string, metadataKey: string): boolean {
    return !this.jsonData[reportName].hasOwnProperty(metadataKey);
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
      const missingColumns = columnHeaders.filter(
        (column, index) => !values[index].trim()
      );
      if (missingColumns.length > 0) {
        const errorMessage = `Missing value(s) for column(s) "${missingColumns.join(
          ', '
        )}" in report "${reportName}" and metadata "${metadataKey}".`;
        this.columnValueErrors.push(errorMessage);
      }
    }
  }

  paginatedReports(): string[] {
    const startIndex = this.currentReportPage * this.reportsPerPage;
    const endIndex = startIndex + this.reportsPerPage;
    return this.reportNames.slice(startIndex, endIndex);
  }

  onReportsPerPageChange() {
    this.currentReportPage = 0;
  }

  // onReportPageChange(event: PageEvent) {
  //   this.currentReportPage = event.pageIndex;
  // }

  // onMetadataPageChange(
  //   event: PageEvent,
  //   reportName: string,
  //   metadataKey: string
  // ) {
  //   if (!this.metadataPageIndex[reportName]) {
  //     this.metadataPageIndex[reportName] = {};
  //   }
  //   this.metadataPageIndex[reportName][metadataKey] = event.pageIndex;
  // }

  getTableData(reportName: string, metadataKey: string): any[] {
    const rows = this.getRows(reportName, metadataKey);
    const pageIndex =
      this.metadataPageIndex[reportName] &&
      this.metadataPageIndex[reportName][metadataKey]
        ? this.metadataPageIndex[reportName][metadataKey]
        : 0;
    const startIndex =
      pageIndex * this.metadataPageSize[reportName][metadataKey];
    const endIndex =
      startIndex + this.metadataPageSize[reportName][metadataKey];
    return rows.slice(startIndex, endIndex);
  }
}



recursive-component.component.html

<div class="table-responsive">
  <ng-container *ngFor="let reportName of reportNames">
    <ng-container *ngIf="!isReportNameMissing(reportName)">
      <div *ngFor="let metadatakey of jsonData[reportName] | keyvalue">
        <div *ngIf="!isMetadataMissing(reportName, metadatakey.key)">
          <div *ngIf="!isColumnHeaderMissing(reportName, metadatakey.key)">
            <table class="table">
              <thead>
                <tr>
                  <th>Report Name</th>
                  <th>Metadata</th>
                  <th
                    *ngFor="
                      let header of getColumnHeaders(
                        reportName,
                        metadatakey.key
                      )
                    "
                  >
                    {{ header }}
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr
                  *ngFor="
                    let row of getPaginatedRows(reportName, metadatakey.key)
                  "
                >
                  <td>{{ reportName }}</td>
                  <td>{{ metadatakey.key }}</td>
                  <td *ngFor="let value of row.split(','); let i = index">
                    <span *ngIf="value.trim()">{{ value.trim() }}</span>
                  </td>
                </tr>
              </tbody>
            </table>
            <div class="pagination">
              <button
                [disabled]="getCurrentPage(reportName, metadatakey.key) === 1"
                (click)="
                  goToPage(
                    reportName,
                    metadatakey.key,
                    getCurrentPage(reportName, metadatakey.key) - 1
                  )
                "
              >
                Previous
              </button>
              <button
                [disabled]="
                  getCurrentPage(reportName, metadatakey.key) ===
                  getTotalPages(reportName, metadatakey.key)
                "
                (click)="
                  goToPage(
                    reportName,
                    metadatakey.key,
                    getCurrentPage(reportName, metadatakey.key) + 1
                  )
                "
              >
                Next
              </button>
            </div>
          </div>
        </div>
      </div>
    </ng-container>
  </ng-container>
</div>





int totalCount = 177;
int erroneousRecordCount = 1;

string errorMessage = $"Error: The response contains an inaccurate data point.{Environment.NewLine}Erroneous record count: {erroneousRecordCount}";
Label1.Text = errorMessage;





using System;
using System.Timers;
using System.Windows.Forms;

namespace InactivityTimeoutExample
{
    public partial class Form1 : Form
    {
        private const double InactivityTimeout = 12 * 60 * 60 * 1000; // 12 hours in milliseconds
        private Timer inactivityTimer;
        private DateTime lastActivityTime;

        public Form1()
        {
            InitializeComponent();
            InitializeInactivityTimer();
            Application.Idle += Application_Idle;
        }

        private void InitializeInactivityTimer()
        {
            inactivityTimer = new Timer(InactivityTimeout);
            inactivityTimer.Elapsed += InactivityTimer_Elapsed;
            inactivityTimer.AutoReset = false;
            ResetInactivityTimer();
        }

        private void ResetInactivityTimer()
        {
            lastActivityTime = DateTime.Now;
            inactivityTimer.Stop();
            inactivityTimer.Start();
        }

        private void InactivityTimer_Elapsed(object sender, ElapsedEventArgs e)
        {
            Application.Exit();
        }

        private void Application_Idle(object sender, EventArgs e)
        {
            if ((DateTime.Now - lastActivityTime).TotalMilliseconds >= InactivityTimeout)
            {
                Application.Exit();
            }
        }

        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            Application.Idle -= Application_Idle;
            inactivityTimer.Stop();
            inactivityTimer.Dispose();
        }
    }
}






using System;
using System.Timers;
using System.Windows.Forms;

namespace InactivityTimeoutExample
{
    public partial class Form1 : Form
    {
        private const double InactivityTimeout = 12 * 60 * 60 * 1000; // 12 hours in milliseconds
        private Timer inactivityTimer;
        private DateTime lastActivityTime;

        public Form1()
        {
            InitializeComponent();
            InitializeInactivityTimer();
        }

        private void InitializeInactivityTimer()
        {
            inactivityTimer = new Timer(InactivityTimeout);
            inactivityTimer.Elapsed += InactivityTimer_Elapsed;
            inactivityTimer.AutoReset = false;
            ResetInactivityTimer();
        }

        private void ResetInactivityTimer()
        {
            lastActivityTime = DateTime.Now;
            inactivityTimer.Stop();
            inactivityTimer.Start();
        }

        private void InactivityTimer_Elapsed(object sender, ElapsedEventArgs e)
        {
            Application.Exit();
        }

        protected override void OnMouseMove(MouseEventArgs e)
        {
            base.OnMouseMove(e);
            ResetInactivityTimer();
        }

        protected override void OnKeyPress(KeyPressEventArgs e)
        {
            base.OnKeyPress(e);
            ResetInactivityTimer();
        }

        // Add more event handlers for other user interactions if needed

        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            inactivityTimer.Stop();
            inactivityTimer.Dispose();
        }
    }
}






using System;
using System.Windows.Forms;

namespace SlidingExpirationExample
{
    public partial class Form1 : Form
    {
        private const int ExpirationDays = 30; // Set the desired expiration period in days

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            // Get the installation date (you can store this in a configuration file or registry)
            DateTime installationDate = GetInstallationDate();

            // Calculate the expiration date based on the installation date and expiration period
            DateTime expirationDate = installationDate.AddDays(ExpirationDays);

            // Check if the current date is beyond the expiration date
            if (DateTime.Now > expirationDate)
            {
                MessageBox.Show("The application has expired.", "Expiration", MessageBoxButtons.OK, MessageBoxIcon.Warning);
                Application.Exit();
            }
            else
            {
                // The application is still valid
                // Perform your desired actions here
            }
        }

        private DateTime GetInstallationDate()
        {
            // Retrieve the installation date from a configuration file or registry
            // For demonstration purposes, we'll use a hardcoded date
            return new DateTime(2023, 1, 1);
        }
    }
}







Summarization: The tool will generate concise and coherent summaries of the meeting by identifying the most relevant information and condensing it into easily digestible minutes.
Action Item Detection: The AI Teams Voice Recorder will automatically detect and list action items, including tasks assigned to specific team members and their respective deadlines.
Integration: The tool will seamlessly integrate with popular productivity tools and platforms like Slack, Trello, or Asana, allowing easy sharing of meeting minutes and action items with team members.
Security and Privacy: The solution will prioritize data security and privacy, ensuring that meeting recordings and transcriptions are stored securely and accessible only to authorized team members.
Benefits:
The AI Teams Voice Recorder will significantly enhance team productivity and collaboration by:

Saving time and effort spent on manual note-taking and meeting documentation.
Providing accurate and comprehensive records of meetings, reducing the risk of missing important information or decisions.
Enabling team members who were unable to attend the meeting to quickly catch up by reviewing the generated minutes.
Facilitating easy sharing and follow-up on action items and tasks assigned during the meeting.
Improving transparency and accountability within the team by maintaining a clear record of discussions and decisions.
By leveraging AI technologies, the AI Teams Voice Recorder will revolutionize the way teams capture and document meetings, ultimately leading to improved efficiency, better decision-making, and enhanced collaboration.

Copy
Retry









public static class XmlExtensions
{
    public static int GetSkipErrorFrtbSensitivitiesCount(this string xmlResponse)
    {
        var xmlDoc = new XmlDocument();
        xmlDoc.LoadXml(xmlResponse);

        var reportNode = xmlDoc.SelectSingleNode("//Report");
        if (reportNode != null)
        {
            var countAttr = reportNode.Attributes["skip_error_frtb_sensitivities_count"];
            if (countAttr != null)
            {
                return int.Parse(countAttr.Value);
            }
        }

        return 0;
    }
}

int skipCount = xmlResponse.GetSkipErrorFrtbSensitivitiesCount();

Certainly, here is the updated section:

Subject: Uniformity Required in API Response Formats

Dear Satya,

I hope this message finds you well.

I have observed an inconsistency in the API responses concerning the formatting of the "Responsible" column values. There are discrepancies in the presentation of names; for instance, there is a variation such as "Jain, Saurabh" versus "Kiran Paramane". It is essential that the API ensures uniformity in data presentation, as this is not an issue that should be corrected on the UI side.

To illustrate, the "IRC Report Metadata" section correctly formats the name as follows:
"Check ABS DAR(0) > 20% ABS Notional, 568850195_SAUDI1USNACBD_LTD, RECOURSE SPV OPTION, Excluded, This rule does not apply to Gap Option trades. Source IDR ladders are being used for IRC and the Notional/MV does not impact the calculation of DAR., Kiran Paramane,"

However, the "Exclusions_APAC Credit" section shows the inconsistency that needs addressing:
"Check Migration Delta is not 0 or NULL if Mig Type <> 'Non-Linear', SAUDI1USNACBD_RWO_RISK, ANNUITY_20211220_USD, Excluded, MV is very small, "Jain, Saurabh", "FALSE""

I trust you will look into this matter and make the necessary adjustments to standardize the API response format.

Thank you for your prompt resolution of this issue.

Best regards,

[Your Name]
[Your Position]









using System;
using System.Xml;

public static class XmlExtensions
{
    public static string GetCobDate(this string xmlResponse)
    {
        // Create an XmlDocument object
        XmlDocument xmlDoc = new XmlDocument();

        // Load the XML response into the XmlDocument
        xmlDoc.LoadXml(xmlResponse);

        // Get the <Report> element
        XmlElement reportElement = (XmlElement)xmlDoc.SelectSingleNode("//Report");

        // Read the value of the cob_date attribute
        string cobDate = reportElement.GetAttribute("cob_date");

        return cobDate;
    }
}

class Program
{
    static void Main()
    {
        string xmlResponse = @"<?xml version=""1.0"" encoding=""UTF-8""?>
<Replies>
  <GetAdhocReportResultReply client_ref=""MarsEnquiryTool-nkuma152"" run_id=""oid57314303503051"" status=""ok"">
    <Report cob_date=""20231208"" report_name=""Method 1 final rating 1.xml [11 Mar 2024 02:27:01 PM]"" user_name=""nkuma152"" request_time=""11-Mar-2024 08:57:10UTC"" create_time=""11-Mar-2024 08:57:11UTC"">
      <!-- Rest of the XML content -->
    </Report>
  </GetAdhocReportResultReply>
</Replies>";

        // Call the extension method to get the cob_date value
        string cobDate = xmlResponse.GetCobDate();

        // Print the cob_date value
        Console.WriteLine("COB Date: " + cobDate);
    }
}










tbody tr {
  display: block;
  white-space: nowrap;
  overflow-x: auto;
}

tbody tr td {
  display: inline-block;
  white-space: normal;
}




.scrollable-cell {
  overflow-x: auto;
  white-space: nowrap;
}


<table *ngIf="!isColumnHeaderMissing(reportName, metadataKey.key)" class="table">
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
      <!-- Create a scrollable container for the cells with many values -->
      <td>
        <div class="scrollable-cell">
          <ng-container *ngFor="let value of row.split(','); let i = index">
            <span *ngIf="value.trim()">{{ value.trim() }}</span>
            <span *ngIf="!value.trim()" class="error-message">
              Missing value for column "{{ getColumnHeaders(reportName, metadataKey.key)[i] }}"
            </span>
          </ng-container>
        </div>
      </td>
    </tr>
  </tbody>
</table>




.table-responsive-custom {
  overflow-x: auto;
  -webkit-overflow-scrolling: touch;
}



import { MatTableDataSource } from '@angular/material/table';
import { MatPaginator } from '@angular/material/paginator';
import { ViewChild } from '@angular/core';


dataSource: MatTableDataSource<any>;


ngOnInit() {
  this.dataSource = new MatTableDataSource(this.getRows(this.currentReportName, this.currentMetadataKey));
}



@ViewChild(MatPaginator, { static: true }) paginator: MatPaginator;

ngAfterViewInit() {
  this.dataSource.paginator = this.paginator;
}



getTableData(reportName: string, metadataKey: string): any[] {
  return this.getRows(reportName, metadataKey);
}


updateDataSource(reportName: string, metadataKey: string) {
  this.dataSource.data = this.getTableData(reportName, metadataKey);
}


<table mat-table [dataSource]="dataSource">
  <!-- Column definitions go here -->
</table>
<mat-paginator [pageSizeOptions]="[5, 10, 20]" showFirstLastButtons></mat-paginator>


















import { MatSelectModule } from '@angular/material/select';

@NgModule({
  imports: [
    // other modules
    MatSelectModule,
  ],
  // other module properties
})
export class AppModule { }




<div id="tipStep6" class="border-boxs">
  <div>
    <mat-form-field>
      <mat-label>Reports per page</mat-label>
      <mat-select [(ngModel)]="reportsPerPage" (selectionChange)="onReportsPerPageChange()">
        <mat-option value="5">5</mat-option>
        <mat-option value="10">10</mat-option>
        <mat-option value="20">20</mat-option>
        <mat-option value="50">50</mat-option>
      </mat-select>
    </mat-form-field>
  </div>

  <div *ngFor="let reportName of paginatedReports">
    <h2>{{ reportName }}</h2>

    <div *ngFor="let metadataKey of jsonData[reportName] | keyvalue">
      <h3>{{ metadataKey.key }}</h3>

      <table mat-table [dataSource]="getTableData(reportName, metadataKey.key)" class="mat-elevation-z8">
        <ng-container *ngFor="let column of getColumnHeaders(reportName, metadataKey.key)" [matColumnDef]="column">
          <th mat-header-cell *matHeaderCellDef>{{ column }}</th>
          <td mat-cell *matCellDef="let row">{{ row[column] }}</td>
        </ng-container>

        <tr mat-header-row *matHeaderRowDef="getColumnHeaders(reportName, metadataKey.key)"></tr>
        <tr mat-row *matRowDef="let row; columns: getColumnHeaders(reportName, metadataKey.key);"></tr>
      </table>

      <mat-paginator [length]="getTableData(reportName, metadataKey.key).length"
                     [pageSize]="metadataPageSize"
                     [pageSizeOptions]="[5, 10, 25, 100]"
                     (page)="onMetadataPageChange($event, reportName, metadataKey.key)">
      </mat-paginator>
    </div>
  </div>

  <mat-paginator [length]="reportNames.length"
                 [pageSize]="reportsPerPage"
                 [pageSizeOptions]="[5, 10, 20, 50]"
                 (page)="onReportPageChange($event)">
  </mat-paginator>
</div>


import { Component, OnInit } from '@angular/core';
import { PageEvent } from '@angular/material/paginator';

@Component({
  selector: 'app-dynamic-table',
  templateUrl: './dynamic-table.component.html',
  styleUrls: ['./dynamic-table.component.css']
})
export class DynamicTableComponent implements OnInit {
  // ...

  reportsPerPage = 10;
  currentReportPage = 0;
  metadataPageSize = 5;
  metadataPageIndex: { [key: string]: { [key: string]: number } } = {};

  // ...

  get paginatedReports(): string[] {
    const startIndex = this.currentReportPage * this.reportsPerPage;
    const endIndex = startIndex + this.reportsPerPage;
    return this.reportNames.slice(startIndex, endIndex);
  }

  onReportsPerPageChange() {
    this.currentReportPage = 0;
  }

  onReportPageChange(event: PageEvent) {
    this.currentReportPage = event.pageIndex;
  }

  onMetadataPageChange(event: PageEvent, reportName: string, metadataKey: string) {
    if (!this.metadataPageIndex[reportName]) {
      this.metadataPageIndex[reportName] = {};
    }
    this.metadataPageIndex[reportName][metadataKey] = event.pageIndex;
  }

  getTableData(reportName: string, metadataKey: string): any[] {
    const rows = this.getRows(reportName, metadataKey);
    const pageIndex = this.metadataPageIndex[reportName] && this.metadataPageIndex[reportName][metadataKey] ? this.metadataPageIndex[reportName][metadataKey] : 0;
    const startIndex = pageIndex * this.metadataPageSize;
    const endIndex = startIndex + this.metadataPageSize;
    return rows.slice(startIndex, endIndex);
  }

  // ...
}



export class DynamicTableComponent implements OnInit {
  // ...
  reportsPerPage = 10;
  currentReportPage = 1;
  totalReportPages: number[] = [];

  // ...

  get paginatedReports(): string[] {
    const startIndex = (this.currentReportPage - 1) * this.reportsPerPage;
    const endIndex = startIndex + this.reportsPerPage;
    return this.reportNames.slice(startIndex, endIndex);
  }

  onReportsPerPageChange() {
    this.currentReportPage = 1;
    this.calculateTotalReportPages();
  }

  goToReportPage(page: number) {
    this.currentReportPage = page;
  }

  calculateTotalReportPages() {
    const totalPages = Math.ceil(this.reportNames.length / this.reportsPerPage);
    this.totalReportPages = Array(totalPages).fill(0).map((_, index) => index + 1);
  }

  ngOnInit() {
    // ...
    this.calculateTotalReportPages();
  }

  // ...
}




<div id="tipStep6" class="border-boxs">
  <div>
    <label for="reportsPerPage">Reports per page:</label>
    <select id="reportsPerPage" [(ngModel)]="reportsPerPage" (change)="onReportsPerPageChange()">
      <option value="5">5</option>
      <option value="10">10</option>
      <option value="20">20</option>
      <option value="50">50</option>
    </select>
  </div>

  <div *ngFor="let reportName of paginatedReports">
    <!-- ... (keep the existing code for displaying each report) ... -->

    <div *ngIf="!isMetadataMissing(reportName, metadataKey.key)">
      <!-- ... -->
      <div class="pagination" *ngIf="!isColumnHeaderMissing(reportName, metadataKey.key)">
        <button *ngFor="let page of [].constructor(getTotalPages(reportName, metadataKey.key)); let i = index"
                (click)="goToPage(reportName, metadataKey.key, i + 1)">
          {{ i + 1 }}
        </button>
      </div>
    </div>
  </div>

  <div class="report-pagination">
    <button *ngFor="let page of totalReportPages" (click)="goToReportPage(page)">
      {{ page }}
    </button>
  </div>
</div>



currentPage: { [key: string]: { [key: string]: number } } = {};

goToPage(reportName: string, metadataKey: string, page: number) {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  this.currentPage[reportName][metadataKey] = page;
}


goToPage(reportName: string, metadataKey: string, page: number) {
  if (!this.currentPage[reportName]) {
    this.currentPage[reportName] = {};
  }
  this.currentPage[reportName][metadataKey] = page;
}

getPaginatedRows(reportName: string, metadataKey: string): any[] {
  const rows = this.getRows(reportName, metadataKey);
  const currentPage = this.currentPage[reportName] && this.currentPage[reportName][metadataKey] ? this.currentPage[reportName][metadataKey] : 1;
  const startIndex = (currentPage - 1) * this.itemsPerPage;
  const endIndex = startIndex + this.itemsPerPage;
  return rows.slice(startIndex, endIndex);
}



<button *ngFor="let page of [].constructor(getTotalPages(reportName, metadataKey.key)); let i = index"
                  (click)="goToPage(reportName, metadataKey.key, i + 1)">
            {{ i + 1 }}
          </button>



#tipStep6 {
  max-width: 1160px;
  overflow-x: auto;
}

.table-container {
  max-width: 100%;
  overflow-x: auto;
}

table {
  width: auto;
  max-width: 100%;
  border-collapse: collapse;
}

th, td {
  /* ... */
  white-space: nowrap;
}



<div *ngFor="let reportName of reportNames">
  <!-- ... -->
  <div *ngIf="!isMetadataMissing(reportName, metadataKey.key)">
    <h3>{{ metadataKey.key }}</h3>
    <!-- ... -->
    <div class="table-container" *ngIf="!isColumnHeaderMissing(reportName, metadataKey.key)">
      <table>
        <!-- ... -->
      </table>
    </div>
    <!-- ... -->
  </div>
  <!-- ... -->
</div>



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

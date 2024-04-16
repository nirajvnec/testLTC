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

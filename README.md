  }
}
Please provide an input that would generate a response containing multiple MAS FRS hierarchies, such as "COMMODITY" and "COUNTRY" in the example above. This will help me ensure that my code handles the display of multiple hierarchies effectively.
Thank you in advance for your assistance. If you have any questions or need further clarification, please don't hesitate to let me know.
Best regards,
[Your Name]





@import './styles/variables';

// .left-area
.left-area {
  width: 232px;
  position: absolute;
  top: 0;
  left: 0;
  height: calc(100%);
  overflow-y: auto;
  background-color: $darkgreen;
}

// .blue-bar
.blue-bar {
  background-color: $greenColor;
  height: 40px;
  line-height: 40px;
  color: $whiteColor;
  padding: 0 24px;
  font-size: 12px;
  font-weight: bold;
  text-transform: uppercase;
}

// .nav-list
.nav-list {
  background-color: $darkgreen;
  li {
    position: relative;
    .item {
      position: relative;
      padding-left: 63px;
      padding-right: 26px;
      display: block;
      height: 64px;
      line-height: 1.4;
      width: 100%;
      z-index: 10;
      display: flex;
      align-items: center;
      border-bottom: 1px solid #1676;

      .txt {
        font-size: 15px;
        color: $whiteColor;
      }

      &:hover, &.current {
        opacity: 1;
        background-color: $whiteColor;

        .txt {
          color: $darkgreen;
        }

        .icons {
          color: $darkgreen; /* Change icon color on hover/active */
        }
      }
    }

    .icons {
      position: absolute;
      left: 20px;
      top: 50%;
      transform: translateY(-50%);
      height: 32px;
      width: 32px;
      color: $whiteColor; /* Ensure icons are white */
      /* Alternatively, if $whiteColor is not defined, use the hex code for white */
      /* color: #ffffff; */
    }
  }
}

.icon-trans {
  display: none;
}

.icon-impact {
  background-position: -256px 0;
}

.icon-re {
  background-position: -320px 0;
}

.icon-flags {
  background-position: -32px -32px;
}

.icon-collections {
  background-position: -96px -32px;
}

.icon-expansion {
  background-position: -160px -32px;
}

.icon-composite {
  background-position: -224px -32px;
}

.icon-report {
  background-position: -288px -32px;
}

.icon-ancestral {
  background-position: 0 -64px;
}

.icon-mars {
  background-position: -64px -64px;
}





"styles": [
  "node_modules/@fortawesome/fontawesome-free/css/all.min.css",
  "src/styles.css"
]


add the Font Awesome CSS file to your project's global styles file (e.g., styles.css):
@import '~@fortawesome/fontawesome-free/css/all.min.css';



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

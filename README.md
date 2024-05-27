Summary:
Issue with MET Freezing During 24.1.4 Release for Specific Reports

Description:
During the 24.1.4 release last Saturday, the Mars Enquiry Tool (MET) experienced a freezing issue when running specific reports. This required a restart of the application and initiation of a new session.
<div *ngFor="let report of jsonParsableReports">
  <h3>{{ report.name }}</h3>
  <table class="table table-bordered">
    <thead>
      <tr>
        <th *ngFor="let key of getKeys(report.jsonString)">{{ key }}</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td *ngFor="let key of getKeys(report.jsonString)">{{ getJsonValue(report.jsonString, key) }}</td>
      </tr>
    </tbody>
  </table>
</div>


jsonParsableReports: {name: string, jsonString: string}[] = [];

ngOnInit() {
  // Assuming you already have the API response stored in this.jsonData
  const reportNames = Object.keys(this.jsonData);
  reportNames.forEach(name => {
    const reportData = this.jsonData[name];
    const reportKeys = Object.keys(reportData);
    reportKeys.forEach(key => {
      const jsonString = reportData[key]?.[1];
      if (this.isJsonString(jsonString)) {
        this.jsonParsableReports.push({name: `${name} - ${key}`, jsonString});
      }
    });
  });
}

isJsonString(str: string): boolean {
  try {
    JSON.parse(str);
  } catch (e) {
    return false;
  }
  return true;
}

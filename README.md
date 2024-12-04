Newly Added Lines
Variables for Grouped Data and Selected Radio State:


groupedResultsData: { [key: string]: ResultSet[] } = {}; // Grouped data by reportName
selectedResultSet: { [key: string]: string } = {}; // Selected radio value for each group
Call to Grouping Method:


this.groupBatchResultsByReportName();
Grouping Logic:


groupBatchResultsByReportName(): void {
  this.groupedResultsData = this.batchResultsData.reduce((groups, item) => {
    const groupName = item.reportName || 'Unknown'; // Default key if reportName is null/undefined
    if (!groups[groupName]) {
      groups[groupName] = [];
    }
    groups[groupName].push(item);
    return groups;
  }, {});
}
Collect Selected Radio Values:

const selectedIds = Object.values(this.selectedResultSet); // Collect selected radio val

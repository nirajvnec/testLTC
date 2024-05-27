getCurrentHierarchiesItems(): {name: string, jsonString: string}[] {
  const startIndex = (this.currentHierarchiesPage - 1) * this.hierarchiesPerPage;
  const endIndex = startIndex + this.hierarchiesPerPage;
  return this.jsonParsableReports.filter(report => report.name === 'MAS FRS Hierarchies').slice(startIndex, endIndex);
}

previousHierarchiesPage(): void {
  if (this.currentHierarchiesPage > 1) {
    this.currentHierarchiesPage--;
  }
}

nextHierarchiesPage(): void {
  const totalPages = this.totalHierarchiesPages();
  if (this.currentHierarchiesPage < totalPages) {
    this.currentHierarchiesPage++;
  }
}

totalHierarchiesPages(): number {
  const hierarchiesCount = this.jsonParsableReports.filter(report => report.name === 'MAS FRS Hierarchies').length;
  return Math.ceil(hierarchiesCount / this.hierarchiesPerPage);
}

getHierarchy(jsonString: string): string {
  const parsedJson = JSON.parse(jsonString);
  return parsedJson.hierarchy;
}

getCountryName(jsonString: string): string {
  const parsedJson = JSON.parse(jsonString);
  return parsedJson.name;
}

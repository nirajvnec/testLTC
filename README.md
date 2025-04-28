async getSelectedTableInfo(tableName: string): Promise<IColumnsInfo[]> {
  return await this.fetchColumnsFromApi(tableName);
}


export interface IAttributeConfigurationService {
  getAllConfigTables(): Promise<Array<IConfigTable>>;
  getSelectedTableInfo(name: string): Promise<Array<IColumnsInfo>>;
  getSelectedTableColumnData(name: string): Promise<Array<Record<string, any>>>;
}


export interface IConfigTable {
  tableName: string;
  displayName?: string;
}

export interface IColumnsInfo {
  name: string;
  displayName?: string;
}


export interface IAttributeConfigurationService {
  getAllConfigTables(): Promise<Array<IConfigTable>>;
  getSelectedTableInfo(name: string): Promise<Array<IColumnsInfo>>;
  getSelectedTableColumnData(name: string): Promise<Array<Record<string, any>>>;
}

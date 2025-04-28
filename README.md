export interface IColumnsInfo {
  name: string;
  displayName?: string;
  type?: string;
  length?: number;
  numberOfDecimal?: number;
  isRequired?: boolean;
  editable?: boolean;
}




import { IAttributeConfigurationService } from '../IAttributeConfigurationService';
import { IColumnsInfo, IConfigTable } from '../models/AttributeConfigurationModels';

export class AttributeConfigurationServiceMock implements IAttributeConfigurationService {

  async getAllConfigTables(): Promise<IConfigTable[]> {
    return [
      { tableName: 'reference.attribute_filter_master_key', displayName: 'Attribute Filter Master Key' },
      { tableName: 'reference.business_owner_region', displayName: 'Business Owner Region' },
      { tableName: 'reference.user', displayName: 'User Table' }, // <-- renamed and added
    ];
  }

  async getSelectedTableInfo(tableName: string): Promise<IColumnsInfo[]> {
    if (tableName === 'reference.attribute_filter_master_key') {
      return [
        { name: 'attribute_filter_master_key', displayName: 'Filter Key' },
        { name: 'data_model_attribute_key', displayName: 'Data Model Key' },
        { name: 'is_active', displayName: 'Active' },
        { name: 'created_at', displayName: 'Created At' },
        { name: 'created_by', displayName: 'Created By' },
        { name: 'attribute_name', displayName: 'Attribute Name' },
      ];
    } else if (tableName === 'reference.business_owner_region') {
      return [
        { name: 'region_id', displayName: 'Region ID' },
        { name: 'owner_name', displayName: 'Owner Name' },
        { name: 'is_primary', displayName: 'Primary Owner' },
        { name: 'modified_at', displayName: 'Modified At' },
      ];
    } else if (tableName === 'reference.user') { // <-- renamed
      return [
        { name: 'user_id', displayName: 'User ID', type: 'number', length: 10, isRequired: true, editable: false },
        { name: 'username', displayName: 'Username', type: 'string', length: 50, isRequired: true, editable: true },
        { name: 'email', displayName: 'Email', type: 'string', length: 100, isRequired: true, editable: true },
        { name: 'created_on', displayName: 'Created On', type: 'date', isRequired: false, editable: false }
      ];
    } else {
      return [];
    }
  }

  async getSelectedTableColumnData(tableName: string): Promise<Record<string, any>[]> {
    if (tableName === 'reference.attribute_filter_master_key') {
      return [
        {
          attribute_filter_master_key: 1,
          data_model_attribute_key: 4723,
          is_active: 1,
          created_at: null,
          created_by: null,
          attribute_name: 'Stress Magnitude',
        },
        {
          attribute_filter_master_key: 2,
          data_model_attribute_key: null,
          is_active: 0,
          created_at: '2019-04-28T20:42:49.000Z',
          created_by: 'yechuriv',
          attribute_name: 'Subdesk Business Owner Region',
        }
      ];
    } else if (tableName === 'reference.business_owner_region') {
      return [
        {
          region_id: 101,
          owner_name: 'Alice',
          is_primary: true,
          modified_at: '2022-01-10T12:30:00Z',
        },
        {
          region_id: 102,
          owner_name: 'Bob',
          is_primary: false,
          modified_at: null,
        }
      ];
    } else if (tableName === 'reference.user') { // <-- renamed
      return [
        {
          user_id: 1001,
          username: 'john_doe',
          email: 'john.doe@example.com',
          created_on: '2024-01-10T09:00:00Z',
          is_active: true
        },
        {
          user_id: 1002,
          username: 'jane_smith',
          email: 'jane.smith@example.com',
          created_on: '2023-12-15T14:30:00Z',
          is_active: false
        },
        {
          user_id: 1003,
          username: 'alice.wonder',
          email: 'alice.wonder@example.com',
          created_on: '2023-11-20T12:00:00Z',
          is_active: true
        },
        {
          user_id: 1004,
          username: 'bob.builder',
          email: 'bob.builder@example.com',
          created_on: null,
          is_active: true
        }
      ];
    } else {
      return [];
    }
  }
}
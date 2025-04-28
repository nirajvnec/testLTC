async getSelectedTableInfo(tableName: string): Promise<IColumnsInfo[]> {
  return await this.fetchColumnsFromApi(tableName);
}


private async fetchColumnsFromApi(tableName: string): Promise<IColumnsInfo[]> {
  if (tableName === 'reference.attribute_filter_master_key') {
    return [
      { name: 'attribute_filter_master_key', displayName: 'Filter Key' },
      { name: 'data_model_attribute_key', displayName: 'Data Model Key' },
      { name: 'is_active', displayName: 'Active' },
      { name: 'created_at', displayName: 'Created At' },
      { name: 'created_by', displayName: 'Created By' },
      { name: 'attribute_name', displayName: 'Attribute Name' }
    ];
  }
  else if (tableName === 'reference.business_owner_region') {
    return [
      { name: 'region_id', displayName: 'Region ID' },
      { name: 'owner_name', displayName: 'Owner Name' },
      { name: 'is_primary', displayName: 'Primary Owner' },
      { name: 'modified_at', displayName: 'Modified At' }
    ];
  }
  else if (tableName === 'reference.user') {
    return [
      { name: 'user_id', displayName: 'User ID', type: 'number', length: 10, isRequired: true, editable: false },
      { name: 'username', displayName: 'Username', type: 'string', length: 50, isRequired: true, editable: true },
      { name: 'email', displayName: 'Email', type: 'string', length: 100, isRequired: true, editable: true },
      { name: 'created_on', displayName: 'Created On', type: 'date', isRequired: false, editable: false },
      { name: 'is_active', displayName: 'Is Active', type: 'string', isRequired: false, editable: false },
      { name: 'first_name', displayName: 'First Name', type: 'string', length: 50, isRequired: true, editable: true },
      { name: 'last_name', displayName: 'Last Name', type: 'string', length: 50, isRequired: true, editable: true },
      { name: 'phone_number', displayName: 'Phone Number', type: 'string', length: 20, isRequired: false, editable: true },
      { name: 'role', displayName: 'Role', type: 'string', length: 30, isRequired: true, editable: true },
      { name: 'last_login', displayName: 'Last Login', type: 'date', isRequired: false, editable: false }
    ];
  }
  else {
    return [];
  }
}

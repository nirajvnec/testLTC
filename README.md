private async fetchColumnsFromApi(tableName: string): Promise<IColumnsInfo[]> {
  if (tableName === 'reference.attribute_filter_master_key') {
    return [
      { name: 'attribute_filter_master_key', displayName: 'Filter Key', displayOrder: 1 },
      { name: 'data_model_attribute_key', displayName: 'Data Model Key', displayOrder: 2 },
      { name: 'is_active', displayName: 'Active', displayOrder: 3 },
      { name: 'created_at', displayName: 'Created At', displayOrder: 4 },
      { name: 'created_by', displayName: 'Created By', displayOrder: 5 },
      { name: 'attribute_name', displayName: 'Attribute Name', displayOrder: 6 }
    ];
  }
  else if (tableName === 'reference.business_owner_region') {
    return [
      { name: 'region_id', displayName: 'Region ID', displayOrder: 1 },
      { name: 'owner_name', displayName: 'Owner Name', displayOrder: 2 },
      { name: 'is_primary', displayName: 'Primary Owner', displayOrder: 3 },
      { name: 'modified_at', displayName: 'Modified At', displayOrder: 4 }
    ];
  }
  else if (tableName === 'reference.user') {
    return [
      { name: 'user_id', displayName: 'User ID', type: 'number', length: 10, isRequired: true, editable: false, displayOrder: 1 },
      { name: 'username', displayName: 'Username', type: 'string', length: 50, isRequired: true, editable: true, displayOrder: 2 },
      { name: 'email', displayName: 'Email', type: 'string', length: 100, isRequired: true, editable: true, displayOrder: 3 },
      { name: 'created_on', displayName: 'Created On', type: 'date', isRequired: false, editable: false, displayOrder: 4 },
      { name: 'is_active', displayName: 'Is Active', type: 'string', isRequired: false, editable: false, displayOrder: 5 },
      { name: 'first_name', displayName: 'First Name', type: 'string', length: 50, isRequired: true, editable: true, displayOrder: 6 },
      { name: 'last_name', displayName: 'Last Name', type: 'string', length: 50, isRequired: true, editable: true, displayOrder: 7 },
      { name: 'phone_number', displayName: 'Phone Number', type: 'string', length: 20, isRequired: false, editable: true, displayOrder: 8 },
      { name: 'role', displayName: 'Role', type: 'string', length: 30, isRequired: true, editable: true, displayOrder: 9 },
      { name: 'last_login', displayName: 'Last Login', type: 'date', isRequired: false, editable: false, displayOrder: 10 }
    ];
  }
  else {
    return [];
  }
}
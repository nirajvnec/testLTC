async getSelectedTableColumnData(tableName: string): Promise<Record<string, any>[]> {
  return await this.fetchTableDataFromApi(tableName);
}

private async fetchTableDataFromApi(tableName: string): Promise<Record<string, any>[]> {
  if (tableName === 'reference.attribute_filter_master_key') {
    const attributes: Record<string, any>[] = [];
    for (let i = 1; i <= 100; i++) {
      attributes.push({
        attribute_filter_master_key: 5000 + i,
        data_model_attribute_key: 6000 + i,
        is_active: i % 2 === 0 ? 'YES' : 'NO',
        created_at: `2023-12-${(i % 28) + 1}T08:00:00Z`,
        created_by: `creator_${i}`,
        attribute_name: `Attribute_${i}`
      });
    }
    return attributes;
  }
  else if (tableName === 'reference.business_owner_region') {
    const regions: Record<string, any>[] = [];
    for (let i = 1; i <= 100; i++) {
      regions.push({
        region_id: 2000 + i,
        owner_name: `Owner_${i}`,
        is_primary: i % 2 === 0 ? 'YES' : 'NO',
        modified_at: `2024-02-${(i % 28) + 1}T11:30:00Z`
      });
    }
    return regions;
  }
  else if (tableName === 'reference.user') {
    const users: Record<string, any>[] = [];
    for (let i = 1; i <= 100; i++) {
      users.push({
        user_id: 1000 + i,
        username: `user_${i}`,
        email: `user${i}@example.com`,
        created_on: `2024-01-${(i % 28) + 1}T09:00:00Z`,
        is_active: i % 2 === 0 ? 'YES' : 'NO',
        first_name: `FirstName${i}`,
        last_name: `LastName${i}`,
        phone_number: `90000000${i.toString().padStart(2, '0')}`,
        role: i % 3 === 0 ? 'Admin' : (i % 3 === 1 ? 'User' : 'Manager'),
        last_login: `2024-04-${(i % 28) + 1}T10:00:00Z`
      });
    }
    return users;
  }
  else {
    return [];
  }
}
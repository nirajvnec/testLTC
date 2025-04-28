else if (tableName === 'reference.user') {
  return [
    { name: 'user_id', displayName: 'User ID', type: 'number', length: 10, isRequired: true, editable: false },
    { name: 'username', displayName: 'Username', type: 'string', length: 50, isRequired: true, editable: true },
    { name: 'email', displayName: 'Email', type: 'string', length: 100, isRequired: true, editable: true },
    { name: 'created_on', displayName: 'Created On', type: 'date', isRequired: false, editable: false },
    { name: 'is_active', displayName: 'Is Active', type: 'boolean', isRequired: true, editable: true },
    { name: 'first_name', displayName: 'First Name', type: 'string', length: 50, isRequired: true, editable: true },
    { name: 'last_name', displayName: 'Last Name', type: 'string', length: 50, isRequired: true, editable: true },
    { name: 'phone_number', displayName: 'Phone Number', type: 'string', length: 20, isRequired: false, editable: true },
    { name: 'role', displayName: 'Role', type: 'string', length: 30, isRequired: true, editable: true },
    { name: 'last_login', displayName: 'Last Login', type: 'date', isRequired: false, editable: false }
  ];
}
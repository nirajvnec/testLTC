else if (tableName === 'reference.user') {
  const users: Record<string, any>[] = [];

  for (let i = 1; i <= 60; i++) {
    users.push({
      user_id: 1000 + i,
      username: `user_${i}`,
      email: `user${i}@example.com`,
      created_on: `2024-01-${(i % 30) + 1}T09:00:00Z`, // Spread dates over January
      is_active: i % 2 === 0, // Alternate active/inactive
      first_name: `FirstName${i}`,
      last_name: `LastName${i}`,
      phone_number: `90000000${i.toString().padStart(2, '0')}`,
      role: (i % 3 === 0) ? 'Admin' : (i % 3 === 1) ? 'User' : 'Manager',
      last_login: `2024-04-${(i % 30) + 1}T10:00:00Z`
    });
  }

  return users;
}
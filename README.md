else if (tableName === 'reference.user') {
  const users: Record<string, any>[] = [];

  for (let i = 1; i <= 60; i++) {
    users.push({
      user_id: 1000 + i,                                 // 1001, 1002, ...
      username: `user_${i}`,                             // user_1, user_2, ...
      email: `user${i}@example.com`,                     // user1@example.com
      created_on: `2024-01-${(i % 28) + 1}T09:00:00Z`,    // Jan 1 - Jan 28
      is_active: i % 2 === 0,                            // true/false alternate
      first_name: `FirstName${i}`,                       // FirstName1, etc
      last_name: `LastName${i}`,                         // LastName1, etc
      phone_number: `90000000${i.toString().padStart(2, '0')}`, // Proper 10-digit number
      role: (i % 3 === 0) ? 'Admin' : (i % 3 === 1) ? 'User' : 'Manager', // Rotate role
      last_login: `2024-04-${(i % 28) + 1}T10:00:00Z`     // April 1 - 28
    });
  }

  return users;
}
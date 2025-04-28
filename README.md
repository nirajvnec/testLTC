else if (tableName === 'reference.attribute_filter_master_key') {
  const attributes: Record<string, any>[] = [];

  for (let i = 1; i <= 60; i++) {
    attributes.push({
      attribute_filter_master_key: 5000 + i,
      data_model_attribute_key: 6000 + i,
      is_active: i % 2 === 0 ? 'YES' : 'NO', // YES / NO instead of true/false
      created_at: `2023-12-${(i % 28) + 1}T08:00:00Z`,
      created_by: `creator_${i}`,
      attribute_name: `Attribute_${i}`
    });
  }

  return attributes;
}
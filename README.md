else if (tableName === 'reference.business_owner_region') {
  const regions: Record<string, any>[] = [];

  for (let i = 1; i <= 60; i++) {
    regions.push({
      region_id: 2000 + i,
      owner_name: `Owner_${i}`,
      is_primary: i % 2 === 0 ? 'YES' : 'NO', // YES / NO format like you want
      modified_at: `2024-02-${(i % 28) + 1}T11:30:00Z`
    });
  }

  return regions;
}
else if (tableName === 'reference.owner_region') {
  const ownerRegions: Record<string, any>[] = [];

  for (let i = 1; i <= 60; i++) {
    ownerRegions.push({
      region_id: 3000 + i,
      owner_name: `OwnerRegion_${i}`,
      is_primary: i % 2 === 0 ? 'YES' : 'NO', // YES / NO
      modified_at: `2024-03-${(i % 28) + 1}T14:45:00Z`
    });
  }

  return ownerRegions;
}
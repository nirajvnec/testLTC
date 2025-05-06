const levelMap: Record<string, string> = {
  '0': 'UBS Group',
  '1': 'Business Group',
  '2': 'Business Unit',
  '3': 'Business Area',
  '4': 'Sector',
  '5': 'Segment',
  '6': 'Function',
  '7': 'Desk',
  '8': 'Subdesk'
};

const onLevelChange = (selectedId: string) => {
  const levelName = levelMap[selectedId] || '';
  setLevel(levelName);

  // Any other logic you want to trigger based on the new level
};
const onLevelChange = async (levelName: string) => {
  try {
    const data = await props.getDataByLevel(levelName);
    // handle success...
  } catch (err: any) {
    props.onError?.([`Failed to fetch data for ${levelName}: ${err.message}`]);
  }
};
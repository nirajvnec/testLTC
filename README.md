export const getAggClasses = async (givenMetric: string): Promise<AggregationClass[]> => {
  try {
    const response = await fetch(`/api/aggregation-classes?metric=${givenMetric}`, {
      method: 'GET',
      headers: { 'Content-Type': 'application/json' },
    });
    if (!response.ok) throw new Error(`Failed to fetch aggregation classes: ${response.statusText}`);
    return await response.json();
  } catch (error) {
    console.error('Error fetching aggregation classes:', error);
    return [];
  }
};


interface AggregationClass {
  display: string;
  value: string;
  relevantAttributes: { display: string; value: string }[];
}

const [aggregationClasses, setAggregationClasses] = useState<AggregationClass[]>([]);
const [loading, setLoading] = useState(false);
const [error, setError] = useState<string | null>(null);

useEffect(() => {
  const fetchAggregationClasses = async () => {
    if (!props.tabData.metric) {
      setAggregationClasses([]);
      return;
    }

    setLoading(true);
    setError(null);

    try {
      const classes = await getAggClasses(props.tabData.metric);
      setAggregationClasses(classes);
    } catch (err) {
      setError('Failed to load aggregation classes.');
    } finally {
      setLoading(false);
    }
  };

  fetchAggregationClasses();
}, [props.tabData.metric]);

// In the render:
{props.tabData.metric && (
  <>
    {loading && <p>Loading aggregation classes...</p>}
    {error && <p>Error: {error}</p>}
    {!loading && !error && aggregationClasses.length > 0 && (
      <Dropdown
        id="aggregation-class"
        labelBeforeSelect="Select a Agg Class"
        labelAfterSelect="Selected Agg Class"
        onChange={onAggClassChange}
        selectedItemId={props.tabData.aggregationClass}
        required
      >
        {renderDropdownListItems(aggregationClasses, 'value', 'display')}
      </Dropdown>
    )}
  </>
)}

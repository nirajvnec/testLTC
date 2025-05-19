export default function SBMImpactAnalysis() {
  const rootContext = useRootContext();
  const summaryTableRef = useRef<any>();
  const segments = ["FX Linear", "FX FRTB IRT", "Segment Example"];
  const [cobDate, setCobDate] = useState('');
  const [currCobDate, setCurrCobDate] = useState('');
  const [selectedSegments, setSelectedSegments] = useState<Array<any>>([]);
  const [riskFactorClass, setRiskFactorClass] = useState('delta');
  const [runWithPositions, setRunWithPositions] = useState(false);
  const [selectedSegmentTab, setSelectedSegmentTab] = useState('');
  const [selectedPositionTab, setSelectedPositionTab] = useState('');
  const [dataTabs, setDataTabs] = useState<DataTabs>({});
  const [result, setResult] = useState<Array<ResultData>>([]);
  const [panelStates, setPanelStates] = useState<any>({});

  const updateSelectedSegments = (value: any) => {
    setDataTabs((prev: DataTabs) => {
      const updatedDataTabs = { ...prev };
      // logic to remove old segments
      return updatedDataTabs;
    });

    if (value.length >= 1) {
      setSelectedSegmentTab(value[value.length - 1]);
    } else {
      setRunWithPositions(false);
    }

    setSelectedSegments(value);
  };

  const onPositionAdd = (segmentName: string) => {
    const newPositionKey = dataTabs[segmentName]?.length + 1 || 2;
    const newTabData: TabData = {
      metric: 'DELTA',
      aggregationClass: '',
      relevantAttributes: { risk: '' },
    };
    const newPos: PositionTab = {
      key: newPositionKey,
      icon: Account16px,
      title: `Position ${newPositionKey}`,
      content: SBMImpactAnalysisTabContent({
        segmentName,
        positionKey: newPositionKey,
        tabData: newTabData,
        updateTabData,
      }),
      segmentName,
      tabData: newTabData,
      isValid: validCheck(newTabData),
    };

    setDataTabs((prev: DataTabs) => ({
      ...prev,
      [segmentName]: [...(prev[segmentName] || []), newPos],
    }));
    setSelectedPositionTab(newPositionKey.toString());
  };

  const deletePosition = (segment: string, key: string) => {
    setDataTabs((prev: DataTabs) => {
      const updatedSegment = (prev[segment] || []).filter((pos) => pos.key !== parseInt(key));
      const renamedSegment = updatedSegment.map((pos, index) => ({
        ...pos,
        key: index + 1,
        title: `Position ${index + 1}`,
        content: SBMImpactAnalysisTabContent({
          segmentName: segment,
          positionKey: index + 1,
          tabData: pos.tabData,
          updateTabData,
        }),
      }));
      return { ...prev, [segment]: renamedSegment };
    });
  };

  const updateTabData = (segmentName: string, positionKey: number, newData: TabData, isValid: boolean) => {
    const newPos: PositionTab = {
      key: positionKey,
      icon: Account16px,
      title: `Position ${positionKey}`,
      content: SBMImpactAnalysisTabContent({
        segmentName,
        positionKey,
        tabData: newData,
        updateTabData,
      }),
      segmentName,
      tabData: newData,
      isValid,
    };

    setDataTabs((prev: DataTabs) => {
      const updatedSegment = (prev[segmentName] || []).map((pos) =>
        pos.key === positionKey ? newPos : pos
      );
      return { ...prev, [segmentName]: updatedSegment };
    });
  };
}
const [readOnly, setReadOnly] = useState(false);
const [navigationUrl, setNavigationUrl] = useState<string | null>(null);

function editReportConfig() {
    const reportConfig = getSelectedRowData();
    
    if (reportConfig && reportConfig.length > 0) {
        setSelectedReportData(reportConfig[0]);
        setShowReportConfiguration(true);
        console.log('setReadonly(true);');
        setReadOnly(true); // Set state, but don't navigate immediately

        const preverifeKey = reportConfig[0].id;
        const newUrl = ConfigurePreverficationReportUrl.replace(':id', preverifeKey);
        console.log(newUrl);

        // Save the URL in state so we can use it in useEffect
        setNavigationUrl(newUrl);
    } else {
        rootContext.showInfo('Please select a report to edit!');
    }
}

// useEffect to handle navigation AFTER state updates
useEffect(() => {
    if (readOnly && navigationUrl) {
        console.log("Navigating to:", navigationUrl);
        history.push(navigationUrl);
    }
}, [readOnly, navigationUrl]);

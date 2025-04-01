useEffect(() => {
    if (readOnly && navigationUrl) {
        console.log("Navigating to:", navigationUrl);
        history.push({
            pathname: navigationUrl,
            state: { readOnly }
        });
    }
}, [readOnly, navigationUrl]);


const location = useLocation();
const readOnly = location.state?.readOnly ?? props.readOnly;

console.log('ConfigureReport props:', props);
console.log('ConfigureReport readOnly:', readOnly);

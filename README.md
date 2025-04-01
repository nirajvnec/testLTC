interface RouteState {
    readOnly?: boolean;
}

function ConfigureReport(props: ConfigureReportProps) {
    const location = useLocation<RouteState>();
    const readOnly = location.state?.readOnly ?? props.readOnly;

    console.log('ConfigureReport props:', props);
    console.log('ConfigureReport readOnly:', readOnly);
}

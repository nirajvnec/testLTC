const [rollToDateLabel, setRollToDateLabel] = useState<string>('');
const [rollFromDateLabel, setRollFromDateLabel] = useState<string>('');


useEffect(() => {
  if (props.toDate) {
    setRollToDateLabel(Utilities.formatMMDDYYYYorYYYYMMDD(props.toDate));
  }
  if (props.fromDate) {
    setRollFromDateLabel(Utilities.formatMMDDYYYYorYYYYMMDD(props.fromDate));
  }
}, [props.toDate, props.fromDate]);

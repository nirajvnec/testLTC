const [formattedFromDate, setFormattedFromDate] = useState<string>('');
const [formattedToDate, setFormattedToDate] = useState<string>('');


setFormattedFromDate(moment(data, 'YYYY-MM-DD').format('DD-MMM-YYYY'));
setFormattedToDate(moment(data, 'YYYY-MM-DD').format('DD-MMM-YYYY'));

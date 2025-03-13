const [columnDefs] = useState([
  {
    field: 'eventTimeStamp',
    headerName: 'Event Time Stamp',
    width: 190,
    filter: 'agTextColumnFilter',
    sortable: true,
    filterParams: {
      buttons: ['reset', 'apply'],
    },
    cellStyle: () => textColumnStyle,

    // 🔥 Conditionally format only if needed
    valueGetter: (params) => {
      const timestamp = params.data?.eventTimeStamp;
      if (!timestamp) return ''; // Handle empty cases

      // Check if it's already in 'DD-MMM-YYYY h:mm:ss A' format
      const parsedDate = moment(timestamp, 'DD-MMM-YYYY h:mm:ss A', true);
      if (parsedDate.isValid()) {
        return timestamp; // Already formatted correctly
      }

      // Convert if it's not already formatted
      return moment(timestamp, 'M/D/YYYY h:mm:ss A').format('DD-MMM-YYYY h:mm:ss A');
    }
  },
]);

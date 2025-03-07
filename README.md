import moment from "moment";

const fromCobDateChange = (value: any) => {
    console.log("Raw Value from DatePicker:", value);

    if (!value) return;

    const selectedDate =
        value instanceof Date
            ? value
            : typeof value === "string"
            ? moment(value, "DD-MMM-YYYY").toDate()
            : null;

    if (!selectedDate) {
        console.error("Unexpected date format:", value);
        return;
    }

    // Format to DD-MMM-YYYY before storing in state
    const formattedDate = moment(selectedDate).format("DD-MMM-YYYY");
    setFromCobDate(formattedDate);

    console.log("Formatted Date for State:", formattedDate);
};

const toCobDateChange = (value: any) => {
    if (!value) return;

    const selectedDate =
        value instanceof Date
            ? value
            : typeof value === "string"
            ? moment(value, "DD-MMM-YYYY").toDate()
            : null;

    if (!selectedDate) {
        console.error("Unexpected date format:", value);
        return;
    }

    const formattedDate = moment(selectedDate).format("DD-MMM-YYYY");
    setToCobDate(formattedDate);
};

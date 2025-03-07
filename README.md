import moment from "moment";

const fromCobDateChange = (value: any) => {
    console.log("Raw Value from DatePicker:", value);

    if (!value) return;

    let selectedDate;
    
    // If value is a Date object, use it directly, else try parsing
    if (value instanceof Date) {
        selectedDate = value;
    } else if (typeof value === "string") {
        selectedDate = moment(value, "DD-MMM-YYYY").toDate();
    } else {
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

    let selectedDate = value instanceof Date ? value : moment(value, "DD-MMM-YYYY").toDate();
    const formattedDate = moment(selectedDate).format("DD-MMM-YYYY");

    setToCobDate(formattedDate);
};

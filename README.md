import moment from "moment";

const fromCobDateChange = (value: any) => {
    if (!value) return;

    // Check if `value` is a Date object or an event
    const selectedDate = value instanceof Date ? value : value?.target?.value;

    // Convert to DD-MMM-YYYY format before storing
    const formattedDate = moment(selectedDate).format("DD-MMM-YYYY");

    setFromCobDate(formattedDate);
    console.log("fromCobDateChange:", formattedDate);
};

const toCobDateChange = (value: any) => {
    if (!value) return;

    const selectedDate = value instanceof Date ? value : value?.target?.value;
    const formattedDate = moment(selectedDate).format("DD-MMM-YYYY");

    setToCobDate(formattedDate);
};

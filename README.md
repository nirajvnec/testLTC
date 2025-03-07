import moment from "moment";

const fromCobDate = "07-Mar-2024"; // Original date in DD-MMM-YYYY format

const fromCobDateISO = moment(fromCobDate, "DD-MMM-YYYY").format("YYYY-MM-DD");

console.log(fromCobDateISO); // Output: 2024-03-07

import moment from "moment";

const rawDateTime = "1/28/2025 1:05:36 PM"; // Non-ISO format

const formattedDateTime = moment(rawDateTime, "M/D/YYYY h:mm:ss A").format("YYYY-MM-DD HH:mm:ss");

console.log(formattedDateTime); // Output: 2025-01-28 13:05:36

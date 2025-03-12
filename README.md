
import React, { useState } from "react";
import DatePicker from "react-datepicker";
import "react-datepicker/dist/react-datepicker.css";

const DateSelector = () => {
  // Keep fromCobDate as a string (ISO format)
  const [fromCobDate, setFromCobDate] = useState("");

  const dayAfterTomorrow = new Date();
  dayAfterTomorrow.setDate(dayAfterTomorrow.getDate() + 2);

  return (
    <DatePicker
      selected={fromCobDate ? new Date(fromCobDate) : null} // Convert string to Date
      onChange={(date) => setFromCobDate(date ? date.toISOString().split("T")[0] : "")} // Convert Date to string
      maxDate={dayAfterTomorrow}
      dateFormat

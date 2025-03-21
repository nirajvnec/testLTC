import React, { useState } from "react";
import DatePicker from "react-datepicker";
import "react-datepicker/dist/react-datepicker.css";

const HistoricalCoBRequest = () => {
    const [fromCobDate, setFromCobDate] = useState(null);
    const [reason, setReason] = useState("");

    const handleDateChange = (date) => {
        setFromCobDate(date);
    };

    const handleSubmit = () => {
        alert("Request Submitted!");
    };

    return (
        <div className="container">
            <h2>Request for Historical CoB Smartclose Email</h2>

            <div className="form-group">
                <label htmlFor="fromDatePicker">Select a Date</label>
                <DatePicker
                    required
                    name="fromDatePicker"
                    selected={fromCobDate}
                    onChange={handleDateChange}
                    className="input-field"
                    dateFormat="yyyy-MM-dd"
                />
            </div>

            <div className="form-group">
                <label htmlFor="reason">Reason</label>
                <input
                    type="text"
                    id="reason"
                    name="reason"
                    className="input-field"
                    value={reason}
                    onChange={(e) => setReason(e.target.value)}
                />
            </div>

            <button onClick={handleSubmit} className="submit-button">
                Request
            </button>
        </div>
    );
};

export default HistoricalCoBRequest;

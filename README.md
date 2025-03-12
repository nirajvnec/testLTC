const moment = require("moment");

const dateString = "1/28/2025 11:55:59 AM";
const formattedDateTime = moment(dateString, "M/D/YYYY h:mm:ss A").format("DD-MM-YYYY HH:mm:ss");

console.log(formattedDateTime); // Output: 28-01-2025 11:55:59




import React, { useState, useEffect } from "react";

const App = () => {
  // State to store the fetched data
  const [data, setData] = useState([]);

  // Function to fetch data from API
  const fetchData = () => {
    fetch("https://jsonplaceholder.typicode.com/posts?_limit=5") // Sample API
      .then((response) => response.json()) // Convert response to JSON
      .then((result) => {
        // Update the state with fetched data
        const transformedData = result.map((item) => ({
          id: item.id,
          title: item.title,
          value: Math.floor(Math.random() * 100), // Adding a random value field
        }));
        setData(transformedData);
      })
      .catch((error) => console.error("Error fetching data:", error)); // Handle errors
  };

  // Fetch data on component mount
  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div>
      <h2>Fetched Data</h2>
      <ul>
        {data.map((item) => (
          <li key={item.id}>
            {item.title}: {item.value}
          </li>
        ))}
      </ul>
      <button onClick={fetchData}>Refresh Data</button>
    </div>
  );
};

export default App;

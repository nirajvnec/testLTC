function processReport(reportName) {
  if (this.jsonData.hasOwnProperty(reportName)) {
    for (let y in this.jsonData[reportName]) {
      if (Array.isArray(this.jsonData[reportName][y]) && this.jsonData[reportName][y].length > 1) {
        try {
          JSON.parse(this.jsonData[reportName][y][1]);
          console.log("Valid JSON found in", reportName);
          delete this.jsonData[reportName];
          console.log("Removed", reportName, "from this.jsonData");
          return true;
        } catch (error) {
          console.log("Invalid JSON in", reportName);
        }
      }
    }
  }
  return false;
}

const reportNames = Object.keys(this.jsonData);

for (let i = 0; i < reportNames.length; i++) {
  const reportName = reportNames[i];
  console.log("Report Name:", reportName);

  const isValidJSON = processReport(reportName);
  if (isValidJSON) {
    console.log("Processed", reportName);
  }
}

console.log("Updated this.jsonData:", this.jsonData);












const response = [
  { id: 1, name: "Object 1", status: "active" },
  { id: 2, name: "Object 2", status: "inactive" },
  { id: 3, name: "Object 3", status: "active" },
  { id: 4, name: "Object 4", status: "inactive" }
];

// Function to filter objects based on status
const filterActiveObjects = (obj: { id: number, name: string, status: string }) => {
  return obj.status !== "inactive";
};

// Filter out objects with status 'inactive'
const filteredObjects = response.filter(filterActiveObjects);

console.log(filteredObjects);





const response = {
  obj1: { id: 1, name: "Object 1", status: "active" },
  obj2: { id: 2, name: "Object 2", status: "inactive" },
  obj3: { id: 3, name: "Object 3", status: "active" },
  obj4: { id: 4, name: "Object 4", status: "inactive" }
};

// Filter out objects with status 'inactive'
const filteredResponse = Object.fromEntries(
  Object.entries(response).filter(([key, obj]) => obj.status !== "inactive")
);

console.log(filteredResponse);







const jsonData = {
  obj1: {},
  obj2: {},
  obj3: {},
  obj4: {}
};

// Convert the object to an array of objects
const objectArray = Object.values(jsonData);

// Now you can use the objectArray as an array of objects
// Iterate over the objects and remove the ones with parsable JSON values
const filteredData = objectArray.filter((obj) => {
  // Specify the property you want to check
  const propertyToCheck = 'someProperty';

  // Check if the object has the specified property
  if (obj.hasOwnProperty(propertyToCheck)) {
    const propertyValue = obj[propertyToCheck];

    // Check if the property value can be parsed as JSON
    if (typeof propertyValue === 'string' && isValidJson(propertyValue)) {
      return false; // Remove the object from the array
    }
  }

  return true; // Keep the object in the array
});

// The filteredData array now contains the objects without parsable JSON values
console.log(filteredData);

// Function to check if a value can be parsed as JSON
function isValidJson(value: string): boolean {
  try {
    JSON.parse(value);
    return true;
  } catch (error) {
    return false;
  }
}











Subject: Compensatory Time Off for 24.1.4 Release Support
Dear Team,
I would like to inform you that I will be taking compensatory time off on May 31, 2024, for the additional hours I invested in supporting the 24.1.4 release on May 25, 2024









Subject: Compensatory Time Off for 24.1.4 Release Support
Dear Team,
I would like to inform you that I will be taking compensatory time off for the additional hours I invested in supporting the 24.1.4 release. Rest 




{
  "MAS FRS Hierarchies": {
    "COMMODITY": [
      {
        "nodes": [
          {
            "title": "USO1BSK",
            "key": "_3",
            "isFolder": false,
            "isLazy": false,
            "tooltip": "",
            "href": "",
            "icon": "",
            "addClass": "",
            "noLink": false,
            "activate": false,
            "focus": false,
            "expand": false,
            "select": false,
            "hideCheckbox": false,
            "unselectable": false,
            "parent_id": 165682658480496,
            "hierarchy": "COMMODITY",
            "name": "USO1BSK",
            "display_name": "USO1BSK",
            "node_id": 165684673555371,
            "parentid": 165682658480496,
            "level": 0,
            "path": "Commodity/. USO1BSK",
            "sequence": 1
          },
          {
            "title": "AIGC LN",
            "key": "_4",
            "isFolder": false,
            "isLazy": false,
            "tooltip": "",
            "href": "",
            "icon": "",
            "addClass": "",
            "noLink": false,
            "activate": false,
            "focus": false,
            "expand": false,
            "select": false,
            "hideCheckbox": false,
            "unselectable": false,
            "parent_id": 165682658480496
          }
        ]
      }
    ],
    "COUNTRY": [
      {
        "nodes": [
          {
            "title": "United States",
            "key": "_1",
            "isFolder": false,
            "isLazy": false,
            "tooltip": "",
            "href": "",
            "icon": "",
            "addClass": "",
            "noLink": false,
            "activate": false,
            "focus": false,
            "expand": false,
            "select": false,
            "hideCheckbox": false,
            "unselectable": false,
            "parent_id": 123456789,
            "hierarchy": "COUNTRY",
            "name": "United States",
            "display_name": "United States",
            "node_id": 987654321,
            "parentid": 123456789,
            "level": 0,
            "path": "Country/United States",
            "sequence": 1
          }
        ]
      }
    ]
  }
}




Subject: Request for Input Resulting in Multiple MAS FRS Hierarchies
Dear [Recipient],
I hope this email finds you well. I am working on displaying JSON data in a structured format, specifically focusing on the "MAS FRS Hierarchies" section. To ensure that my implementation handles multiple hierarchies correctly, I would greatly appreciate if you could provide an input that would generate a response containing multiple MAS FRS hierarchies.
The ideal input should result in a JSON response similar to the following structure:
            
            
            
            "activate": false,
            "focus": false,
            "expand": false,
            "select": false,
            "hideCheckbox": false,
            "unselectable": false,
            "parent_id": 165682658480496
          }
        ]
      }
    ],
    "COUNTRY": [
      {
        "nodes": [
          {
            "title": "United States",
            "key": "_1",
            "isFolder": false,
            "isLazy": false,
            "tooltip": "",
            "href": "",
            "icon": "",
            "addClass": "",
            "noLink": false,
            "activate": false,
            "focus": false,
            "expand": false,
            "select": false,
            "hideCheckbox": false,
            "unselectable": false,
            "parent_id": 123456789,
            "hierarchy": "COUNTRY",
            "name": "United States",
            "display_name": "United States",
            "node_id": 987654321,
            "parentid": 123456789,
            "level": 0,
            "path": "Country/United States",
            "sequence": 1
          }
        ]
      }
    ]
  }
}
Please provide an input that would generate a response containing multiple MAS FRS hierarchies, such as "COMMODITY" and "COUNTRY" in the example above. This will help me ensure that my code handles the display of multiple hierarchies effectively.
Thank you in advance for your assistance. If you have any questions or need further clarification, please don't hesitate to let me know.
Best regards,
[Your Name]

git config --global --unset alias.cmd
git config --global alias.cmdhome '!start cmd /K "cd /d C:\\Users\\nkuma152"'



git config --global alias.lock '!rundll32.exe user32.dll,LockWorkStation'









git config --global alias.vs2022 "!\"C:/Program Files/Microsoft Visual Studio/2022/Community/Common7/IDE/devenv.exe\" ."




-- Assuming you have tables AURASYNCV4RIGHT, AURASYNCV4PERMISSION, and AURASYNCV4LOGIN

-- Get the RIGHTID for '2NDLEVEL SUPPORT READ WRITE'
DECLARE @RightID INT;
SELECT @RightID = RIGHTID 
FROM AURASYNCV4RIGHT 
WHERE RightName = '2NDLEVEL SUPPORT READ WRITE';

-- Get the LOGINID for the user 'nkuma152'
DECLARE @LoginID INT;
SELECT @LoginID = LOGINID 
FROM AURASYNCV4LOGIN 
WHERE LoginName = 'nkuma152';

-- Insert into the AURASYNCV4PERMISSION table
INSERT INTO AURASYNCV4PERMISSION (RIGHTID, LOGINID)
VALUES (@RightID, @LoginID);

-- Optional: if you need to insert into AURASYNCV4RIGHT directly
-- Ensure the combination of RIGHTID and LoginName is unique if required.
INSERT INTO AURASYNCV4RIGHT (RightName)
VALUES ('2NDLEVEL SUPPORT READ WRITE');

-- Insert the user login if it's not already present
INSERT INTO AURASYNCV4LOGIN (LoginName)
VALUES ('nkuma152');




USE YourDatabaseName; -- Replace with your database name

-- Query to get the stored procedure definition
SELECT 
    sm.definition 
FROM 
    sys.sql_modules sm
    INNER JOIN sys.objects o ON sm.object_id = o.object_id
WHERE 
    o.type = 'P'
    AND o.name = 'spsGetAuraV4ITAccess';



Get-OutlookClient | Select-Object SmtpServer


git composeemail "recipient@example.com" "Subject Line" "Email body text."



Set-ExecutionPolicy RemoteSigned -Scope CurrentUser


git config --global alias.composeemail '!powershell -Command "& \"$env:EMAIL_SCRIPT\""'






param (
    [string]$recipient,
    [string]$subject,
    [string]$body
)

# Create a new Outlook Application COM object
$Outlook = New-Object -ComObject Outlook.Application
$Mail = $Outlook.CreateItem(0)

# Set the email properties
$Mail.To = $recipient
$Mail.Subject = $subject
$Mail.Body = $body

# Display the email
$Mail.Display()



[System.Environment]::SetEnvironmentVariable('EMAIL_SCRIPT', 'C:\\Users\\nkumar152\\compose_email.ps1', [System.EnvironmentVariableTarget]::User)


git config --global alias.openvsc '!code .'



git config --global --unset alias.closevscode

git config --global alias.closevscode '!cmd.exe /C \"taskkill /IM Code.exe /F\"'



git config --global --unset alias.closeallcmd

git config --global alias.closeallcmd '!cmd.exe /C \"taskkill /IM cmd.exe /F\"'



git config --global --unset alias.closevst

git config --global alias.closevst '!cmd.exe /C \"taskkill /IM devenv.exe /F\"'


git config --global alias.closevscode '!taskkill /IM Code.exe /F'

git config --global alias.closeallcmd '!taskkill /IM cmd.exe /F'




Oracs Client 12c x86:-
ComponentName=04381 ORADataClil 2cR132,ComponentVersion = 1.2.3_1 ,Packa
geSuffix= Pkg 1 »PackageVersion = 1.2.3_1



I'm glad to hear you had a good break, even though it was short. Spending quality time with your boy must have been wonderful!


How are you? How was your break? Did you find it to be short or long enough?

Set-Alias -Name homedir -Value {cd "C:\Users\nkuma152"}  # Replace path if needed



git config --global alias.homedir '!start cmd.exe /K "cd /d c:/Users/nkuma152"'
git config --global alias.homedir '!start cmd.exe /K \"cd /d c:/Users/nkuma152\"'

git config --global --unset alias.homedir


git config --global alias.homedir '!cmd /C "cd /d c:/Users/nkuma152 && start cmd.exe /K"'

git config --global alias.homedir '!cd /d c:/Users/nkuma152'





git config --global --get alias.homedir



git config --global --unset alias.rmf

git config --global --get alias.rmf

git config --global alias.rmf '!rm -rf'




git config --global alias.rmf = !rm -rf


git config --global alias.met-clone '!git clone https://odyssey.apps.csintra.net/bitbucket/scm/marsgui/marsgui.git && cd marsgui && git checkout'


git config --global --get alias.met-clone




git config --global alias.mer-clone '!git clone https://odyssey.apps.csintra.net/bitbucket/scm/mars/mars_web.git && cd mars_web && git checkout'




git config --global alias.cm "!git add -A && git commit -v"

git config --global alias.un "reset --soft HEAD~1"

git config --global --get alias.cm

git config --global --get alias.un





code %USERPROFILE%\.gitconfig

git config --global alias.scpp "!git stash; git checkout $1; git pull; git stash pop"

git config --global --get-all alias.scpp


git glal feature-branch-name






git config --global alias.su '!git config --global user.name "KUMAR, NIRAJ" && git config --global user.email "niraj.kumar.4@credit-suisse.com"'

git su



git config --global alias.su '!git config --global user.name "KUMAR, NIRAJ" && git config --global user.email "niraj.kumar.4@credit-suisse.com"'

git su





git config --global alias.gl 'config --global --list'

Now you can simply use git gl to run git config --global --list.


alias.ll=config --local --list


Now, you can simply use git ll to run git config --local --list.


git config --global alias.ar 'commit --amend --reset-author'
git config --global alias.arn 'commit --amend --reset-author --no-edit'

Now you can use git ar for git commit --amend --reset-author 

and 

git arn for git commit --amend --reset-author --no-edit.


Subject: Merlin Bug Fix: Data Extraction Logic Correction

Hi Shadab,

Please provide approval for this, as it is scheduled for release in version 24.2.0.

Pull Request #484: MARS-18444: Merlin Dev Test Fix - Odyssey Bitbucket (csintra.net)

Best regards,
Niraj









Could we connect to clarify and avoid any confusion?
I thought we had already merged the Pull Request below. Are you suggesting that we need to seek approval on it, or is there another issue? Please confirm.

Hi John,

The last issue has been resolved and was deployed today. Please provide your feedback.

Best regards,
Niraj

24.2.0.1148

CurrentlyRunningRoListReports.aspx

I don't recall off the top of my head and will need to debug the code.


Remove Risk Parameter "SKIPERRORFRTBSENSITIVITIES" from RRAO Methods


deleteReportsWhichHasJsonParsableReponse(jsonData: ReportData) {
    const reportNames = Object.keys(jsonData);

    for (let i = 0; i < reportNames.length; i++) {
        const reportName = reportNames[i];
        console.log("Report Name:", reportName);

        if (this.isJsonDataValid(jsonData, reportName)) {
            delete jsonData[reportName];
            console.log("Deleted report:", reportName);
        }
    }
}

isJsonDataValid(jsonData: ReportData, reportName: string): boolean {
    let propName = Object.keys(jsonData[reportName]).at(0);
    let data = jsonData[reportName][propName].at(1);
    let isValid: boolean = false;

    try {
        let parsedData = JSON.parse(jsonData[reportName][propName].at(1).toString());
        console.log(parsedData);
        isValid = true;
    } catch (error) {
        console.log(error);
        isValid = false;
    }

    return isValid;
}



const myClass = new MyClass(jsonData);
const value = myClass.getPropertyValue("report1", 1);
console.log(value);










class MyClass {
  private jsonData: { [reportName: string]: { [key: string]: any } };

  constructor(jsonData: { [reportName: string]: { [key: string]: any } }) {
    this.jsonData = jsonData;
  }

  getPropertyValue(reportName: string, index: number): any {
    const propertyName = Object.keys(this.jsonData[reportName])[0] as keyof typeof this.jsonData[typeof reportName];
    return this.jsonData[reportName][propertyName][index];
  }
}

// Usage
const jsonData = {
  "report1": {
    "COMMODITY": ["value1", "value2"]
  },
  "report2": {
    "COUNTRY": ["value3", "value4"]
  }
};

const myClass = new MyClass(jsonData);
const value = myClass.getPropertyValue("report1", 1);
console.log(value); // Output: "value2"




npm install @fortawesome/fontawesome-free





git pull --rebase origin feature-branch && git push origin feature-branch





function processReport(reportName) {
  if (this.jsonData.hasOwnProperty(reportName)) {
    for (let y in this.jsonData[reportName]) {
      if (Array.isArray(this.jsonData[reportName][y]) && this.jsonData[reportName][y].length > 1) {
        const value = this.jsonData[reportName][y][1];
        if (typeof value === 'object' && value !== null) {
          console.log("Valid object found in", reportName);
          delete this.jsonData[reportName];
          console.log("Removed", reportName, "from this.jsonData");
          return true;
        } else {
          console.log("Invalid object in", reportName);
        }
      }
    }
  }
  return false;
}












function processReport(reportName) {
  if (this.jsonData.hasOwnProperty(reportName)) {
    for (let y in this.jsonData[reportName]) {
      if (Array.isArray(this.jsonData[reportName][y]) && this.jsonData[reportName][y].length > 1) {
        try {
          const jsonString = JSON.stringify(this.jsonData[reportName][y][1]);
          JSON.parse(jsonString);
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

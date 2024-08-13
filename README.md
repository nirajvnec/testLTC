## Detailed Steps

### Step 1: Application Initialization
- The Windows Form application is started, initializing all necessary components and settings.

### Step 2: Get User's Name and Domain
- The application retrieves the current user's information using the following environment settings:
  - `Environment.UserName`: Fetches the username of the currently logged-in user.
  - `Environment.UserDomainName`: Fetches the domain name associated with the user.

### Step 3: Retrieve PID using LDAP from AD
- The application queries the Active Directory (AD) to retrieve the Process ID (PID) of the user:
  - Uses `Environment.UserName` and `Environment.UserDomainName` to search the AD for the corresponding PID.
  
### Step 4: Open X509 Certificate Store
- The application opens the X509 certificate store to manage digital certificates:
  - **Search for Valid Certificates**: Scans the certificate store for valid certificates that can be used for authentication.
  - **Prompt User to Select Certificate**: A dialog box is displayed, prompting the user to select an appropriate certificate. Specific instructions are provided depending on whether the environment is PROD or TEST.
  - **Retrieve Selected Certificate**: The application retrieves the certificate selected by the user.
  - **Close Certificate Store**: After the certificate is selected, the certificate store is closed to secure the process.

### Step 5: API Calls (Mars, UDM, Myriad)
- The application makes calls to various APIs using the selected certificate for authentication:
  - **Create HTTP POST Request**: An HTTP POST request is created using the provided connection string as the URI.
  - **Add Client Certificate**: The client certificate is added to the request using the `AddCertificate()` function.
  - **Configure Request Properties**: Several properties of the request are configured, including:
    - Setting the method to POST.
    - Setting a timeout for the request.
    - Enabling automatic decompression for both GZip and Deflate.
  - **Set Content-Type & Request Body**: The content type is set, and the request body is prepared using the `SetContentTypeAndRequest()` function, based on the provided `contentType` parameter.
  - **Enable TCP Keep-Alive**: TCP keep-alive is enabled with specific timeout and interval settings to maintain a persistent connection.

## Conclusion
- This process ensures that the application is correctly initialized, retrieves user credentials securely, and makes authenticated API calls as needed.


https://marketplace.visualstudio.com/items?itemName=NeVeS.MermaidEditorForVisualStudio&ssr=false#overview 
Mermaid editor for Visual Studio


```mermaid
graph TD
    A[Start] --> B[Initialize Windows Form Application]
    B --> C[Get Environment.UserName and Environment.UserDomainName]
    C --> D[Fetch PID from Active Directory using LDAP]
    D --> D1[Query AD Groups: GBLADHEDANI, CHADHEDANI, EURADHEDANI]
    D1 --> E[Open X509 Certificate Store and Search for Valid Certificates]
    E --> G[Prompt User to Select and Retrieve Certificate]
    G --> I[Close Certificate Store]
    I --> J[Prepare HTTP POST Request]
    J --> K[Add Client Certificate to Request]
    K --> L[Configure Request Properties, including Timeout and Decompression]
    L --> M[Add Headers, including Accept-Encoding, and Set Content Type]
    M --> N[Enable TCP Keep-Alive]
    N --> O{Call API}
    O -->|Mars API| P[Process Mars API Response]
    O -->|UDM API| Q[Process UDM API Response]
    O -->|Myriad API| R[Process Myriad API Response]
    P --> S[End]
    Q --> S
    R --> S

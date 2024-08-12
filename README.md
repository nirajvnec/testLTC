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

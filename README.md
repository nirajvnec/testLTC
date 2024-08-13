```mermaid
flowchart LR
    A[Application Initialization] --> B[Get User's Name and Domain]
    B --> C[Retrieve PID using LDAP from AD]
    C --> F[User apps permissions are fetched by PID To Security Server]
    F --> D[Open X509 Certificate Store]
    D --> E[API Calls Mars, UDM, Myriad]

    subgraph "Step 1: Application Initialization"
    direction TB
    A --> A1[Initialize Application]
    A1 --> A2[Set Up Logging]
    A2 --> A3[Load Configuration]
    end

    subgraph "Step 2: Get User's Name and Domain"
    direction TB
    B --> B1[Environment.UserName]
    B --> B2[Environment.UserDomainName]
    end

    subgraph "Step 3: Retrieve PID using LDAP from AD"
    direction TB
    C --> C1[Connect to Active Directory]
    C1 --> C2[Perform LDAP Query]
    C2 --> C3[Extract PID from LDAP Result]
    end

    subgraph "Step 4: Fetch User Apps Permissions"
    direction TB
    F --> F1[Connect to Security Server]
    F1 --> F2[Send PID to Security Server]
    F2 --> F3[Receive User App Permissions]
    end

    subgraph "Step 5: Open X509 Certificate Store"
    direction TB
    D --> D1[Search for Valid Certificates]
    D1 --> D2[Prompt User to Select Certificate]
    D2 --> D3[Retrieve Selected Certificate]
    D3 --> D4[Close Certificate Store]
    end

    subgraph "Step 6: API Calls Mars, UDM, Myriad"
    direction TB
    E --> E1[Create HTTP POST Request]
    E1 --> E2[Add Client Certificate]
    E2 --> E3[Configure Request Properties]
    E3 --> E4[Set Content-Type & Request Body]
    E4 --> E5[Enable TCP Keep-Alive]
    end

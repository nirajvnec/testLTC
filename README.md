```mermaid
flowchart TD
  A[Application Initialization] --> |Get User's Name and Domain| B
  B --> C[Retrieve PID using LDAP from AD]
  C --> F[User apps permissions are fetched by passing User's Windows NTID to Security Server]
  F --> D[Open X509 Certificate Store]
  D --> E[API Calls Mars, UDM, Myriad]

  subgraph "Step 1: Application Initialization (All Apps)"
    A1[Initialize Application]
    A1 --> A2[Set Up Logging]
    A2 --> A3[Load Configuration]
  end

  subgraph "Step 2: Get User's Name and Domain(Only Windows app)"
    B --> B1[Environment.UserName]
    B2[Environment.UserDomainName]
  end

  subgraph "Step 3: Retrieve PID using LDAP from AD(Only Windows app)"
    C --> C1[Connect to Active Directory]
    C1 --> C2[Perform LDAP Query using NTID]
    C2 --> C3[Extract PID from LDAP Result]
  end

  subgraph "Step 4: Fetch User access"
    F1[Connect to Security Server]
    F1 --> F2[Send UserName i.e. Windows NTID to Security Server]
    F2 --> F3[Receive User App Permissions]
  end

  subgraph "Step 5: Open X509 Certificate Store"
    D1[Search for Valid Certificates]
    D1 --> D2[Prompt User to Select Certificate]
    D2 --> D3[Retrieve Selected Certificate]
    D3 --> D4[Close Certificate Store]
  end

  subgraph "Step 6: API Calls Mars, UDM, Myriad"
    E1[Create HTTP GET/POST Request]
    E1 --> E2[Add Client Certificate]
    E2 --> E3[Configure Request Properties]
    E3 --> E4[Set Content-Type & Request Body]
    E4 --> E5[Enable TCP Keep-Alive]
  end

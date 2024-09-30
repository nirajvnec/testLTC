```mermaid
flowchart TD
  A[Application Initialization] --> B[Get User's Name and Domain]
  B --> C[Retrieve PID using LDAP from AD]
  C --> D[Retrieve User App permission from SecurityNet]
  D --> E[Open X509 Certificate Store]
  E --> F[API Calls Mars, UDM, Myriad]

  subgraph "Step 1: Application Initialization"
    A1[Initialize all components and settings]
  end

  subgraph "Step 2: Get User's Name and Domain"
    B1[Fetch Environment.UserName]
    B1 --> B2[Fetch Environment.UserDomainName]
  end

  subgraph "Step 3: Retrieve PID using LDAP from AD"
    C1[Query Active Directory for PID]
    C2[Use Environment.UserName and Environment.UserDomainName to search PID in AD groups]
    C1 --> C2
  end

  subgraph "Step 4: Retrieve User App permission from SecurityNet"
    D1[Pass UserName or NTID to SecurityNet]
    D2[Fetch User's App permissions]
  end

  subgraph "Step 5: Open X509 Certificate Store"
    E1[Search for valid certificates]
    E1 --> E2[Prompt user to select certificate]
    E2 --> E3[Retrieve selected certificate]
    E3 --> E4[Close certificate store]
  end

  subgraph "Step 6: API Calls (Mars, UDM, Myriad)"
    F1[Create HTTP POST Requests]
    F1 --> F2[Add Client Certificate]
    F2 --> F3[Configure request properties]
    F3 --> F4[Set Content-Type and Request Body]
    F4 --> F5[Enable TCP Keep-Alive]
  end

```mermaid
flowchart TD
    A[Application Initialization] --> B[Get User's Name and Domain]
    B --> C[Retrieve PID using LDAP from AD]
    C --> D[Open X509 Certificate Store]
    D --> E[API Calls Mars, UDM, Myriad]

    subgraph "Step 2: Get User's Name and Domain"
    B --> B1[Environment.UserName]
    B --> B2[Environment.UserDomainName]
    end

    subgraph "Step 4: Open X509 Certificate Store"
    D --> D1[Search for Valid Certificates]
    D1 --> D2[Prompt User to Select Certificate]
    D2 --> D3[Retrieve Selected Certificate]
    D3 --> D4[Close Certificate Store]
    end

    subgraph "Step 5: API Calls Mars, UDM, Myriad"
    E --> E1[Create HTTP POST Request]
    E1 --> E2[Add Client Certificate]
    E2 --> E3[Configure Request Properties]
    E3 --> E4[Set Content-Type & Request Body]
    E4 --> E5[Enable TCP Keep-Alive]
    end

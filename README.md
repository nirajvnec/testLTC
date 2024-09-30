```mermaid
flowchart LR
A[Application Initialization] --> B[Get User's Name and Domain]
C[Retrieve PID using LDAP from AD]
F[User apps permissions are fetched by PID to Security Server]
D[Open X509 Certificate Store]
E[API Calls Mars, UDM, Myriad]

subgraph "<b><font color=green>Step 1: Application Initialization</font></b>"
direction TB
A1 --> A1[Initialize Application]
A2 --> A3[Load Configuration]
end

subgraph "Step 2: Get User's Name and Domain"
direction TB
B1[Environment.UserName]
B2[Environment.UserDomainName]
end

subgraph "Step 3: Retrieve PID using LDAP from AD"
direction TB
C1[Connect to Active Directory]
C2[Perform LDAP Query]
C3[Extract PID from AD using LDAP Result]
end

subgraph "Step 4: Fetch User Apps Permissions"
direction TB
F1[Connect to Security Server]
F2[Send PID to Security Server]
F3[Receive User App Permissions]
end

subgraph "Step 5: Open X509 Certificate Store"
direction TB
D1[Search for Valid Certificates]
D2[Prompt User to Select Certificate]
D3[Retrieve Selected Certificate]
D4[Close Certificate Store]
end

subgraph "Step 6: API Calls Mars, UDM, Myriad"
direction TB
E1[Create HTTP POST Request]
E2[Add Client Certificate]
E3[Configure Request Properties]
E4[Set Content-Type & Request Body]
E5[Enable TCP Keep-Alive]
end

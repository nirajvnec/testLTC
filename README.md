Get-ChildItem -Path Cert:\CurrentUser\My | Select-Object -ExpandProperty Subject



Get-ChildItem -Path Cert:\LocalMachine\My | Where-Object { $_.Subject -match "Your Name" }



Get-ChildItem -Path Cert:\CurrentUser\My | Where-Object { $_.Subject -match "Your Name" }


Get-ChildItem -Path Cert:\LocalMachine\ -Recurse

Get-ChildItem -Path Cert:\CurrentUser\ -Recurse




```mermaid
flowchart TD
    A[Application Initialization] --> B[Get User's Name and Domain]
    B --> C[Retrieve PID using LDAP from AD]
    C --> F[User apps permissions are fetched by PID To Security Server]
    F --> D[Open X509 Certificate Store]
    D --> E[API Calls Mars, UDM, Myriad]

    subgraph "<b><font color=green>Step 1</font></b>"
    direction LR
    A --> A1[Initialize Application]
    A1 --> A2[Set Up Logging]
    A2 --> A3[Load Configuration]
    end

    subgraph "<b><font color=green>Step 2</font></b>"
    direction LR
    B --> B1[Environment.UserName]
    B1 --> B2[Environment.UserDomainName]
    end

    subgraph "<b><font color=green>Step 3</font></b>"
    direction LR
    C --> C1[Connect to Active Directory]
    C1 --> C2[Perform LDAP Query]
    C2 --> C3[Extract PID from LDAP Result]
    end

    subgraph "<b><font color=green>Step 4</font></b>"
    direction LR
    F --> F1[Connect to Security Server]
    F1 --> F2[Send PID to Security Server]
    F2 --> F3[Receive User App Permissions]
    end

    subgraph "<b><font color=green>Step 5</font></b>"
    direction LR
    D --> D1[Search for Valid Certificates]
    D1 --> D2[Prompt User to Select Certificate]
    D2 --> D3[Retrieve Selected Certificate]
    D3 --> D4[Close Certificate Store]
    end

    subgraph "<b><font color=green>Step 6</font></b>"
    direction LR
    E --> E1[Create HTTP POST Request]
    E1 --> E2[Add Client Certificate]
    E2 --> E3[Configure Request Properties]
    E3 --> E4[Set Content-Type & Request Body]
    E4 --> E5[Enable TCP Keep-Alive]
    end

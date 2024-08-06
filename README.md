# Windows Form Application User Information Flow

This document describes the flow of user information retrieval and processing in a Windows Form Application using SmartCard and UserCertificate. The retrieved information is then used to interact with various external systems.

## Flow Steps

1. **Windows Form Application Initiates Process**
   - The Windows Form Application starts the process.

2. **Interaction with SmartCard**
   - The application interacts with the SmartCard to gather user-related information.

3. **Interaction with UserCertificate**
   - Simultaneously, the application uses the UserCertificate to gather additional user-related information.

4. **Retrieve User Information**
   - Information from the SmartCard and UserCertificate is used to retrieve the user information.

5. **Pass Information to Security Server**
   - The retrieved user information is then passed to the Security Server.

6. **Get User Access/Permissions**
   - The Security Server processes the user information to determine and provide user access or permissions.

7. **Attach User Certificate to HTTP Request**
   - The user certificate is attached to HTTP requests for additional operations.

8. **Call External Systems**
   - The application calls various external systems, such as:
     - MARS System
     - UDM
     - Myriad

This flow ensures that user information is securely retrieved and processed, and that necessary permissions are granted before interacting with external systems.

## Diagram

```mermaid
graph TD;
    A[Windows Form Application] --> B[SmartCard];
    A --> C[UserCertificate];
    B --> D[Get User Information];
    C --> D;
    D --> E[Security Server];
    E --> F[Get User Access/Permissions];
    F --> G[HTTP Request with User Certificate];
    G --> H[MARS System];
    G --> I[UDM];
    G --> J[Myriad];





graph TD;
    A[Windows Form Application] --> B[SmartCard];
    A --> C[UserCertificate];
    B --> D[Get User Information];
    C --> D;
    D --> E[Security Server];
    E --> F[Get User Access/Permissions];
    F --> G[HTTP Request with User Certificate];
    G --> H[MARS System];
    G --> I[UDM];
    G --> J[Myriad];








Subject: Assistance Required with User Certificate and GAIA IDP Integration for UBS Dev Cloud Host


Hi Arif,

We are encountering the following issues:

Since the Proof of Concept (POC) uses a User Certificate and we see that the UBS workspace has a User Certificate, we cannot run the POC on the Workspace. We need to use the UBS development cloud host.

Could you please guide us on how to order a User Certificate on the UBS development cloud host and how to establish trust with the GAIA IDP? The POC code required a User Certificate when we ran it.

For Winform applications:
Assume we have a Windows Forms application using OIDC to call GAIA IDP to receive a JWT bearer access token. We must register the application with GAIA IDP to obtain a Client ID, Client Secret, and Authority (the endpoint to call IDP over HTTP to get the access token for a given environment).

We need detailed steps to register and obtain these details.

Will the CS Infrastructure continue or eventually be migrated to UBS infrastructure? Currently, we have LT, FT, UAT, and PROD servers hosted on CS infrastructure. Will these continue, or will they be migrated to UBS infrastructure?

For Web applications:
We have a web app hosted by IIS using Marsnet for app-to-app connections. When it calls the IDP, what kind of certificate will it use, and how will the access token be fetched from GAIA IDP?

Also, what request would we need to raise to register it and obtain the Client ID, Client Secret, and Authority (the endpoint to call IDP over HTTP to get the access token for a given environment)?

We need the exact steps.

Regards,
Niraj




---

---


---
title: App to App flow
---
flowchart TD
    reg1["Register Client Application (Windows Service) with GAIA IDP: Obtain Client ID, Client Secret, and Authority"] --> step1
    reg2["Register Resource Application (ASP.NET Web API) with GAIA IDP: Obtain Resource ID (API Identifier)"] --> step1
    step1["Step 1: Windows Service Initialization"] --> A
    A[Windows Service]
    A -->|"Step 2: Request Token
    Client calls GAIA IDP via HTTP POST to token endpoint:
    - ClientId
    - ClientSecret
    - Scope
    - GrantType
    Requests Access Token"| B
    B[GAIA Identity Provider]
    B -->|"Step 3: Returns Access Token
    JWT containing claims"| A
    A -->|"Step 5: Send API Request
    HTTP GET with Authorization header:
    Bearer {access_token}"| C
    C[ASP.NET Web API]
    C -->|"Step 6: Fetch Metadata
    HTTP GET to well-known endpoint:
    /.well-known/openid-configuration"| D
    D[GAIA Metadata Endpoint]
    D -->|"Step 7: Returns Metadata
    JSON containing endpoint URLs,
    supported features, etc."| C
    C -->|"Step 8: Fetch JWKS
    HTTP GET to JWKS endpoint from metadata"| E
    E[GAIA JWKS Endpoint]
    E -->|"Step 9: Returns JWKS
    JSON Web Key Set for token validation"| C
    C -->|"Step 10: Validate Token
    Verify signature, expiration,
    issuer, audience, etc."| F
    F[Token Validation]
    F -->|"Step 11: Returns Validation Result
    Allow or deny API access"| C
---

```mermaid

flowchart TD
    reg1["Register Client Application (Windows Forms Application) with GAIA IDP: Obtain Client ID, Client Secret, and Tenant ID"] --> step1
    step1["Step 1: Windows Forms Application Initialization"] --> A
    A[Windows Forms Application]
    A -->|"Step 2: Request Token
    Client calls GAIA IDP via HTTP POST to token endpoint:
    - ClientId
    - ClientSecret
    - Scope
    - GrantType
    Requests Access Token"| B
    B[GAIA Identity Provider]
    B -->|"Step 3: Returns Access Token
    JWT containing claims:
    - Issuer
    - Subject
    - Audience
    - Expiry
    - IssuedAt
    - Scopes"| A
    A -->|"Step 4: Store Access Token
    Store the JWT securely in the application"| step4
    step4 -->|"Step 5: Send API Request
    HTTP GET with Authorization header:
    Bearer {access_token}"| C
    C[ASP.NET Web API]
    C -->|"Step 6: Validate Access Token
    API validates the JWT access token"| F
    F[Token Validation]
    F --> F1[Verify Issuer]
    F --> F2[Check Expiration]
    F --> F3[Validate Audience]
    F --> F4[Verify Signature]
    F1 & F2 & F3 & F4 -->|"Valid"| G[Allow Access]
    F1 & F2 & F3 & F4 -->|"Invalid"| H[Deny Access]
    H --> H1[Set 401 Unauthorized]
    H1 --> H2[Prepare Error Response]
    H2 --> H3[Set Error Code]
    H2 --> H4[Set Error Description]
    G --> I[Request Processed]

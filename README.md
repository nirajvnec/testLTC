---

---


---
title: App to App flow
---
flowchart TD
    reg1["Register Client Application (Windows Service) with GAIA IDP: Obtain Client ID, Client Secret, and Tenant ID"] --> step1
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
    A -->|"Step 5: Send API Request
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

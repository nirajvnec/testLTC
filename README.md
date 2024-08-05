flowchart TD
    reg1["Register Client Application (Windows Service) with GAIA IDP: Obtain Client ID, Client Secret, and Tenant ID"] --> step1
    reg2["Register Resource Application (ASP.NET Web API) with GAIA IDP: Obtain Resource ID (API Identifier)"] --> step1
    step1["Step 1: Windows Service Initialization"] --> A
    A[Windows Service]
    A -->|Step 2: Request Token| B
    B[GAIA Identity Provider]
    B -->|Step 3: Returns Access Token| A
    A -->|Step 5: Send API Request with Token| C
    C[ASP.NET Web API]
    C -->|Step 6: Fetch Metadata| D
    D[GAIA Metadata Endpoint]
    D -->|Step 7: Returns Metadata| C
    C -->|Step 8: Fetch JWKS| E
    E[GAIA JWKS Endpoint]
    E -->|Step 9: Returns JWKS| C
    C -->|Step 10: Validate Token| F
    F[Token Validation]
    F -->|Step 11: Returns Validation Result| C# Token Validation Flowchart

```mermaid
flowchart TD
    reg1([Register Client Application (Windows Service) with GAIA IDP: Obtain Client ID, Client Secret, and Tenant ID]) --> step1([Step 1: Windows Service Initialization])
    reg2([Register Resource Application (ASP.NET Web API) with GAIA IDP: Obtain Resource ID (API Identifier)]) --> step1

    step1 --> A[Windows Service]
    A -->|Step 2: Request Token| B[GAIA Identity Provider]
    B -->|Step 3: Returns Access Token| A
    step2([Step 4]) --> A
    A -->|Step 5: Send API Request with Token| C[ASP.NET Web API]
    C -->|Step 6: Fetch Metadata| D[GAIA Metadata Endpoint]
    D -->|Step 7: Returns Metadata| C
    C -->|Step 8: Fetch JWKS| E[GAIA JWKS Endpoint]
    E -->|Step 9: Returns JWKS| C
    C -->|Step 10: Validate Token| F[Token Validation]
    F -->|Step 11: Returns Validation Result| C

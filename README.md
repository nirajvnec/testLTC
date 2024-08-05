# Token Validation Flowchart

```mermaid
flowchart TD
    A[Windows Service] -->|Request Token| B[GAIA Identity Provider]
    B -->|Returns Access Token| A
    A -->|Send API Request with Token| C[ASP.NET Web API]
    C -->|Fetch Metadata| D[GAIA Metadata Endpoint]
    D -->|Returns Metadata| C
    C -->|Fetch JWKS| E[GAIA JWKS Endpoint]
    E -->|Returns JWKS| C
    C -->|Validate Token| F[Token Validation]
    F -->|Returns Validation Result| C

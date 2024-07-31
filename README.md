```mermaid
flowchart TD
    A[Desktop and Web Applications] --> B[Register in GAIA/IDP]
    B --> C[GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments.]
    C --> D[The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint. ]
    D --> E{GAIA Endpoint PROD, UAT, SIT}
    E --> F[GAIA responds with access token]
    F --> G[The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems.]
    G --> H[The API needs to use the access token to retrieve user information such as PID, etc.]

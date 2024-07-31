Hi Adrian,

This POC uses User Certificate to get Access Token, but my doubt is on UBS workspace. Do we have user certificates installed?

I am asking this question because if the user certificate is not selected, this code will throw an exception. Access denied.

However, we thought we were using GAIA/IDP to eliminate user certificates. Please let me know if I am wrong.

Regards
Niraj






{flowchart}
  "Desktop and Web Applications" -> "Register in GAIA/IDP"
  "Register in GAIA/IDP" -> "GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments."
  "GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments." -> "The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint."
  "The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint." -> "GAIA Endpoint PROD, UAT, SIT"
  "GAIA Endpoint PROD, UAT, SIT" -> "GAIA responds with access token"
  "GAIA responds with access token" -> "The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems."
  "The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems." -> "The API needs to use the access token to retrieve user information such as PID, etc."
{flowchart}




```mermaid
flowchart TD
    A[Desktop and Web Applications] --> B[Register in GAIA/IDP]
    B --> C[GAIA provides a unique Client ID and endpoints for the PROD, UAT, and SIT environments.]
    C --> D[The application requests an access token via an HTTP request by providing a registered Client ID and utilizing the GAIA endpoint. ]
    D --> E{GAIA Endpoint PROD, UAT, SIT}
    E --> F[GAIA responds with access token]
    F --> G[The client application passes the access token as an HTTP header to MARS, UDM, MYRIAD, and other systems.]
    G --> H[The API needs to use the access token to retrieve user information such as PID, etc.]

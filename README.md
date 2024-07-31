h1. Client

# *Request Access Token:*
  - Makes an HTTP call to the Identity Provider (IDP) or GAIA.
  - Passes the following parameters:
    - _clientId_
    - _endpoint URL_
    - _redirection URL_
  - Uses OpenID Connect (OIDC) to obtain an access token.

# *Use Access Token:*
  - Attaches the access token as an HTTP header when calling the server API.

h1. Server API (e.g., MARS, UDM, Myriad)

- Uses the access token to:
  - Retrieve user information.
  - Fetch user-specific access details.

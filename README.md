As discussed, please find below the proof of concept (POC) for the client-server communication flow using access tokens:


# Client

1. **Request Access Token:**
   - Makes an HTTP call to the Identity Provider (IDP) or GAIA.
   - Passes the following parameters:
     - `clientId`
     - `endpoint URL`
     - `redirection URL`
   - Uses OpenID Connect (OIDC) to obtain an access token.

2. **Use Access Token:**
   - Attaches the access token as an HTTP header when calling the server API.

# Server API (e.g., MARS, UDM, Myriad)

- Uses the access token to:
  - Retrieve user information.
  - Fetch user-specific access details.

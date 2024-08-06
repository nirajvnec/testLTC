# Windows Form Application User Information Flow

This document describes the flow of user information retrieval and processing in a Windows Form Application using a SmartCard and its certificate. The retrieved information is then used to interact with various external systems.

## Flow Steps

1. **Windows Form Application Initiates Process**
   - The Windows Form Application starts the process.

2. **Retrieve User Information with SmartCard**
   - Information from the SmartCard is used to retrieve the user information.

3. **Attach SmartCard Certificate to HTTP Request**
   - The SmartCard Certificate must be attached to any HTTP request for the TLS handshake. This is crucial for calling the security server, MARS System, UDM, or Myriad. Without this certificate, the request will be rejected.

4. **Pass Information to Security Server**
   - The retrieved user information is then passed to the Security Server.

5. **Get User Access/Permissions**
   - The Security Server processes the user information to determine and provide user access or permissions.

6. **Call External Systems**
   - The application calls various external systems, such as:
     - MARS System
     - UDM
     - Myriad

This flow ensures that user information is securely retrieved and processed, and that necessary permissions are granted before interacting with external systems.

## Diagram

```mermaid
graph TD;
    A[Windows Form Application] --> B[Retrieve User Information with SmartCard];
    B --> C[Attach SmartCard Certificate to HTTP Request];
    C --> D[Security Server];
    D --> E[Get User Access/Permissions];
    C --> F[MARS System];
    C --> G[UDM];
    C --> H[Myriad];

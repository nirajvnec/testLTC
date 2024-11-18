# Negotiate Security Support Provider (SSP)

"Negotiate" is a security support provider (SSP) used in Windows that acts as an intermediary to determine the optimal authentication protocol to use between Kerberos and NTLM.

## Key Points about "Negotiate"

### Protocol Selection
- **Kerberos**: Chosen if both the client and server are in the same Active Directory (AD) domain or in trusted domains. Kerberos is a more secure and efficient protocol.
- **NTLM**: Falls back to NTLM (NT LAN Manager) if Kerberos is not feasible, such as in cases where the client or server is not in an AD domain or in a workgroup environment.

### Functionality
- Negotiate simplifies application development by abstracting the choice of authentication protocol. Developers can use Negotiate without explicitly coding for Kerberos or NTLM.

## Kerberos
- Relies on a trusted third party, typically a Key Distribution Center (KDC), provided by Active Directory.
- Provides mutual authentication and is more secure due to its use of tickets and encryption.

## NTLM
- A challenge-response-based authentication protocol used for older systems or systems not joined to an AD domain.
- Less secure compared to Kerberos, as it is vulnerable to replay and certain brute-force attacks.

## Use Case in Windows
- "Negotiate" is often used in scenarios like:
  - Authenticating to web services.
  - Remote Desktop Protocol (RDP) connections.
  - Accessing shared network resources.

## Mechanism
- When authentication is required:
  1. Negotiate first attempts Kerberos.
  2. If Kerberos cannot be used (e.g., lack of AD configuration), it automatically falls back to NTLM.

This dual-protocol approach ensures compatibility across a wide range of network environments while preferring the more secure Kerberos when possible.

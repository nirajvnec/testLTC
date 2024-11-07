Setup and Authentication Guidelines for Accessing CS Web Applications on UBS Workspace VDI


- **Testing Setup**: Guidelines are in place for accessing PKI and Kerberos-authenticated CS web applications within UBS Workspace VDIs on Azure or Standard environments (Windows 10/11).

- **PKI Requirement**: Ensure a CS User Certificate is installed and available in the UBS Workspace to authenticate securely.

- **Supported Devices**: Only VDIs are supported for testing; physical devices, like Microsoft Surface Pro, are not compatible.

- **Certificate Assignment & Verification**: Designated testers and app owners receive a CS Certificate. Verify both CS and UBS certificates in the Personal Certificate Store.

- **Workspace Build Check**: Confirm the UBS Workspace build version (61 or above for Azure) using the “Machine identifier” to ensure compatibility.

- **Quick Setup for CS App Launcher**: Download and install the CS App Launcher by running `InstallAsUser.cmd` from the `CSRunAs-Package.zip` file, creating a desktop shortcut.

- **Launching Applications**: Drag and drop CS app shortcuts into `CSAppLauncher v1.6.0.0` to initiate applications under the CS authentication context.

- **Authentication Status**: Verify cached Kerberos tickets in the **About** section of CSAppLauncher to confirm successful authentication.

- **Diagnostic Script**: Use `CSRunAs-Diags.ps1` to check prerequisites (certificates, Kerberos Proxy) in PowerShell.

- **CSRunAs Tool**: Run applications with CS authentication using `CSRunAs.exe`, similar to `runas.exe`, with options to launch applications (e.g., Chrome) under CS credentials.

- **Wrapper Package Distribution**: Once the wrapper package is fully distributed to all UBS Workspace clients, manual installation of `CSRunAs-Package.zip` will no longer be required, as the package will handle setup automatically. Until then, the `CSRunAs-Package.zip` needs to be installed manually each session due to the ephemeral nature of UBS Workspace.

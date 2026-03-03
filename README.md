# HelloID-Conn-SA-Full-Microsoft-AD-EntraID-AccountDisable

> [!IMPORTANT]
> This repository contains the connector and configuration code only. The implementer is responsible for acquiring the connection details such as username, password, certificate, etc. You might even need to sign a contract or agreement with the supplier before implementing this connector. Please contact the client's application manager to coordinate the connector requirements.

## Description

HelloID-Conn-SA-Full-Microsoft-AD-EntraID-AccountDisable is a delegated form designed for use with HelloID Service Automation (SA). It can be imported into HelloID and customized according to your requirements.

By using this delegated form, you can disable user accounts in both Active Directory and Microsoft Entra ID. The following options are available:

1. Search and select an AD user (wildcard search by name, display name, UserPrincipalName, or mail)
2. Disable the selected user account in  Active Directory
3. Disable the corresponding user account in Entra ID
4. Option to revoke signin sessions of the account in Entra ID

## Getting started

### Requirements

#### Active Directory Service Account Permissions

The service account used for the Active Directory connection must have permissions to:
- Search and retrieve active user accounts from Active Directory
- Disable user accounts using the `Disable-ADAccount` cmdlet
- Read user properties (DisplayName, Mail, UserPrincipalName, etc.)

#### App Registration & Certificate Setup

Before implementing this connector, make sure to configure a Microsoft Entra ID App Registration. During the setup process, you'll create a new App Registration in the Entra portal, assign the necessary API permissions, and generate and assign a certificate.

Follow the official Microsoft documentation for creating an App Registration and setting up certificate-based authentication:

- [App-only authentication with certificate](https://learn.microsoft.com/en-us/powershell/exchange/app-only-auth-powershell-v2?view=exchange-ps#set-up-app-only-authentication)

#### HelloID-specific configuration

Once you have completed the Microsoft setup and followed their best practices, configure the following HelloID-specific requirements.

- API Permissions (Application permissions):
  - `User.Read.All` - To search and read user information
  - `User.ReadWrite.All` - To disable user accounts
  - `User.RevokeSessions.All` - To revoke user sign-in sessions (if using this feature)
- Certificate Base64 encoded string:
  - Base64 encoded string of the certificate assigned to the app registration. For instructions on creating the certificate and obtaining the base64 string, refer to our forum post: [Setting up a certificate for Microsoft Graph API in HelloID connectors](https://forum.helloid.com/forum/helloid-provisioning/5338-instruction-setting-up-a-certificate-for-microsoft-graph-api-in-helloid-connectors#post5338)

### Connection settings

The following global variables must be configured in HelloID when importing and configuring the delegated form.

| Setting | Description | Mandatory |
| ------- | ----------- | --------- |
| AdUsersEnabledSearchOu | The organizational units (OUs) to search for enabled AD users. Multiple OUs can be specified separated by semicolons (;) | Yes |
| EntraIdTenantId | The unique identifier (ID) of the tenant in Microsoft Entra ID | Yes |
| EntraIdAppId | The unique identifier (ID) of the App Registration in Microsoft Entra ID | Yes |
| EntraIdCertificateBase64String | The Base64-encoded string representation of the app certificate | Yes |
| EntraIdCertificatePassword | The password associated with the app certificate | Yes |

## Remarks

### Dual System Account Disable

- This connector disables accounts in **both** Active Directory and Microsoft Entra ID in a single delegated form action
- The AD account is disabled using the `Disable-ADAccount` cmdlet with the user's ObjectGuid for precise identification
- The Entra ID account is disabled by setting the `accountEnabled` property to `false` via Microsoft Graph API

### User Lookup by ID

- Uses the AD user `ObjectGuid` for correlation and updates to ensure consistency
- Uses the Entra ID user `userPrincipalName` for account updates

### Wildcard Search

- Users can search for accounts using a wildcard (`*`) to return all enabled users, or by entering partial text to search across display name, UserPrincipalName, mail, and name fields
- The search queries Active Directory only; matching is based on AD user attributes

### Account Status Filtering

- The data source only returns users with `Enabled = true` in Active Directory, meaning search results show only active user accounts available for disabling

### Optional Session Revocation

- The form includes an optional toggle to revoke sign-in sessions in Entra ID
- When enabled, this forces the user to re-authenticate on all devices and applications

### Certificate-Based Authentication

- The connector uses certificate-based authentication to generate JSON Web Tokens (JWT) for secure communication with Microsoft Graph API

## Development resources

### API endpoints

The following Microsoft Graph API endpoints are used by the connector:

| Endpoint | Purpose |
| -------- | ------- |
| /v1.0/users/{userPrincipalName} | Update a specific user (disable account) |
| /v1.0/users/{id}/revokeSignInSessions | Revoke user sign-in sessions (optional) |

### PowerShell Cmdlets

The following PowerShell cmdlets are used by the connector:

| Cmdlet | Purpose |
| ------ | ------- |
| Get-ADUser | Retrieve enabled user accounts from Active Directory |
| Disable-ADAccount | Disable user accounts in Active Directory |

### Documentation

For more information on the APIs and PowerShell cmdlets used in this connector, please refer to:

Microsoft Graph API:

- [List users](https://learn.microsoft.com/en-us/graph/api/user-list)
- [Update user](https://learn.microsoft.com/en-us/graph/api/user-update)
- [Revoke sign-in sessions](https://learn.microsoft.com/en-us/graph/api/user-revokesigninsessions)

Active Directory PowerShell:

- [Get-ADUser](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-aduser)
- [Disable-ADAccount](https://docs.microsoft.com/en-us/powershell/module/activedirectory/disable-adaccount)

## Getting help

> [!TIP]
> For more information on Delegated Forms, please refer to our [documentation](https://docs.helloid.com/en/service-automation/delegated-forms.html) pages.

## HelloID docs

The official HelloID documentation can be found at: [https://docs.helloid.com/](https://docs.helloid.com/)

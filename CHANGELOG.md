# Change Log

All notable changes to this project will be documented in this file. The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-03-03

This is the first official release of HelloID-Conn-SA-Full-Microsoft-AD-EntraID-AccountDisable. This release provides functionality to disable user accounts in both Active Directory and Microsoft Entra ID through a single HelloID Service Automation delegated form.

### Added

- Dual system account disable functionality for both Active Directory and Microsoft Entra ID in a single delegated form
- PowerShell data source to search for enabled AD users with wildcard support across Name, DisplayName, UserPrincipalName, and Mail
- Task to disable Active Directory user accounts using the `Disable-ADAccount` cmdlet
- Task to disable Entra ID user accounts using the Microsoft Graph API (`PATCH /users/{userPrincipalName}` with `accountEnabled: false`)
- Optional revoke sign-in sessions functionality in Entra ID when disabling accounts
- Switch/checkbox in dynamic form to control whether to revoke sign-in sessions
- API integration with Microsoft Graph `/users/{id}/revokeSignInSessions` endpoint
- Certificate-based authentication for secure Microsoft Graph API access
- Custom JWT token generation for Entra ID authentication
- Support for multiple AD organizational units (OUs) with semicolon-separated configuration
- Comprehensive error handling with `Resolve-MicrosoftGraphAPIError` function
- Detailed audit logging for all account disable operations (both AD and Entra ID)
- Audit logging for sign-in session revocation operations
- User lookup by ObjectGuid for Active Directory operations
- User lookup by userPrincipalName for Entra ID operations
- All-in-one setup script for automated HelloID form deployment
- Global variable configuration for:
  - `AdUsersEnabledSearchOu` - AD organizational units to search
  - `EntraIdTenantId` - Microsoft Entra ID tenant identifier
  - `EntraIdAppId` - App Registration identifier
  - `EntraIdCertificateBase64String` - Certificate for authentication
  - `EntraIdCertificatePassword` - Certificate password
- Dynamic form with grid selection showing DisplayName, UserPrincipalName, Department, Title, and Description
- Account status filtering to show only enabled AD users available for disabling
- Comprehensive README with setup instructions, requirements, and API documentation

### Changed

### Deprecated

### Removed

### Fixed

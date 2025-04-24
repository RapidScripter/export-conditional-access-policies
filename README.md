# Export Conditional Access Policies to Excel

This PowerShell script exports Microsoft Entra Conditional Access (CA) policies to a CSV file with detailed information, including policy settings, conditions, and controls.

## Features

- 📊 Exports **33 attributes** for detailed CA policy analysis
- 🔍 **Multiple report types**:
  - All CA policies (default)
  - Active (enabled) policies only
  - Disabled policies only
  - Report-only mode policies
  - Recently created policies
  - Recently modified policies
- 🔒 Supports **both interactive and certificate-based authentication**
- ⚡ Automatic **Microsoft Graph module installation**
- 📂 Outputs to CSV with **automatic file open option**

## Prerequisites

- PowerShell 5.1 or later
- Global Administrator or Conditional Access Administrator role
- Required permissions:
  - `Policy.Read.All`
  - `Directory.Read.All` 
  - `Application.Read.All`

## Installation

1. Clone this repository:
   ```powershell
   git clone https://github.com/RapidScripter/export-conditional-access-policies.git
   cd export-conditional-access-policies

2. The script will automatically install the required Microsoft Graph Beta module if missing.

## Usage

### Basic Commands

```powershell
# Interactive MFA session (exports all policies)
.\Export-CAPolicies.ps1

# Filter by policy state
.\Export-CAPolicies.ps1 -ActiveCAPoliciesOnly
.\Export-CAPolicies.ps1 -DisabledCAPoliciesOnly
.\Export-CAPolicies.ps1 -ReportOnlyMode

# Time-based filters
.\Export-CAPolicies.ps1 -RecentlyCreatedCAPolicies 30
.\Export-CAPolicies.ps1 -RecentlyModifiedCAPolicies 15

### Advanced Options

| Parameter               | Description                          | Example                           |
|-------------------------|--------------------------------------|-----------------------------------|
| `-ActiveCAPoliciesOnly` | Export only enabled policies         | `-ActiveCAPoliciesOnly`           |
| `-DisabledCAPoliciesOnly` | Export only disabled policies      | `-DisabledCAPoliciesOnly`         |
| `-ReportOnlyMode`       | Export report-only mode policies     | `-ReportOnlyMode`                 |
| `-RecentlyCreatedCAPolicies` | Policies created within X days  | `-RecentlyCreatedCAPolicies 30`   |
| `-RecentlyModifiedCAPolicies` | Policies modified within X days | `-RecentlyModifiedCAPolicies 15`  |
| `-TenantId`            | Tenant ID for certificate auth       | `-TenantId "xxxxxx-xxxx..."`      |
| `-ClientId`            | App ID for certificate auth          | `-ClientId "xxxxxx-xxxx..."`      |
| `-CertificateThumbprint` | Certificate thumbprint for auth     | `-CertificateThumbprint "A1B2..."`|

## Output

The script generates a CSV report with these columns:

- **CA Policy Name**: Policy display name
- **Description**: Detailed policy description  
- **State**: Enabled/Disabled/EnabledForReportingButNotEnforced
- **Include Users**: Targeted users (All/None/Specific)
- **Exclude Groups**: Excluded security groups
- **Include Applications**: Cloud apps included
- **User Risk Levels**: Required risk thresholds  
- **Grant Controls**: Access requirements (MFA/CompliantDevice)
- **Session Controls**: Persistent browser settings
- **Modified Time**: Last policy update timestamp

Sample output filename: `CA_Policies_Report_2025-04-24_143022.csv`

## Example Output

| CA Policy Name          | State  | Include Users | Exclude Groups      | Include Applications | Grant Controls | Modified Time        |
|-------------------------|--------|---------------|---------------------|----------------------|----------------|----------------------|
| Require MFA for Admins  | Enabled| All           | BreakGlass_Group    | All                  | Require MFA    | 2025-03-15 09:22:11  |
| Block Legacy Auth       | Enabled| All           | None                | Office365            | Block          | 2025-01-10 14:35:47  |
| High Risk User Protection| ReportOnly | All       | SecOps_Team        | All                  | Require MFA    | 2025-04-01 11:05:33  |

## Troubleshooting

| Error/Symptom | Solution |
|--------------|----------|
| "Missing Microsoft Graph module" | Run `Install-Module Microsoft.Graph.Beta` or allow auto-install |
| "Insufficient permissions" | Verify `Policy.Read.All` and `Directory.Read.All` permissions |
| "Certificate authentication failed" | Validate thumbprint and app registration |
| "No policies exported" | Check if filters exclude all policies |

## Best Practices

1. **Baseline Exports**: Maintain monthly policy configuration backups  
2. **Change Monitoring**: Track modifications with `-RecentlyModifiedCAPolicies 7`  
3. **Secure Automation**: Use certificate auth for scheduled tasks  
4. **Policy Validation**: Compare report-only vs enforced policies  
5. **Access Reviews**: Combine with entitlement management reports  

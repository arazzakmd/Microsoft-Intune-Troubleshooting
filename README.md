# Microsoft Intune Troubleshooting Commands

A practical collection of PowerShell and Windows troubleshooting commands for Microsoft Intune, Microsoft Entra ID, MDM enrollment, device compliance, security, and Windows endpoint troubleshooting.

---

## Overview

This repository contains commonly used commands for IT Support Engineers and System Administrators working with Microsoft Intune and Microsoft 365 environments.

The commands are grouped into the following areas:

- Microsoft Entra ID / Device Identity
- Intune / MDM
- Security
- Windows / Hybrid Troubleshooting

> **Important:** Some commands require Administrator privileges. Always review the command and understand its purpose before running it on a production device.

---

# 1. Microsoft Entra ID / Device Identity

## 1.1 Check Microsoft Entra ID Join Status

### Command

```powershell
dsregcmd /status
```

### Purpose

Displays Microsoft Entra ID device registration and join information.

### Useful Information

The output can help troubleshoot:

- Microsoft Entra ID Join
- Hybrid Microsoft Entra ID Join
- Device identity
- Primary Refresh Token (PRT)
- Tenant information
- Authentication status

### Recommended Usage

Run from Command Prompt or PowerShell:

```powershell
dsregcmd /status
```

---

## 1.2 Refresh Primary Refresh Token (PRT)

### Command

```powershell
dsregcmd /refreshprt
```

### Purpose

Attempts to refresh the Primary Refresh Token.

### Useful For

- Authentication troubleshooting
- Microsoft 365 sign-in issues
- PRT-related problems
- Conditional Access troubleshooting

### Example

```powershell
dsregcmd /refreshprt
```

After running the command, you can check the status again:

```powershell
dsregcmd /status
```

---

# 2. Microsoft Intune / MDM Troubleshooting

## 2.1 Open Access Work or School

### Command

```powershell
Start-Process "ms-settings:workplace"
```

### Purpose

Opens the Windows **Access work or school** settings page.

### Useful For

- Checking work/school account connection
- Reviewing device enrollment
- Starting manual synchronization
- Troubleshooting Intune enrollment

---

## 2.2 Restart Intune Management Extension

### Command

```powershell
Restart-Service -Name IntuneManagementExtension
```

### Purpose

Restarts the Microsoft Intune Management Extension service.

### Useful For

- Intune PowerShell script troubleshooting
- Win32 application deployment issues
- Policy processing issues
- Intune Management Extension related problems

### Check Service Status

```powershell
Get-Service -Name IntuneManagementExtension
```

---

## 2.3 Open Intune Management Extension Logs

### Command

```powershell
explorer.exe "C:\ProgramData\Microsoft\IntuneManagementExtension\Logs"
```

### Purpose

Opens the Intune Management Extension log folder.

### Common Logs

The folder contains logs useful for troubleshooting:

- Application deployment
- PowerShell scripts
- Policy processing
- Detection rules
- Remediation
- Intune Management Extension activity

### Log Location

```text
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs
```

---

## 2.4 Collect MDM Diagnostics

### Command

```powershell
mdmdiagnosticstool.exe -area DeviceEnrollment -cab "C:\MDMLogs.cab"
```

### Purpose

Collects MDM diagnostic information into a CAB file.

### Useful For

- Device enrollment troubleshooting
- MDM enrollment issues
- Intune management problems
- Diagnostic log collection

### Output

The example creates:

```text
C:\MDMLogs.cab
```

> **Security Note:** Diagnostic files may contain device or user information. Do not upload production/customer diagnostic files to a public GitHub repository.

---

# 3. Security Troubleshooting

## 3.1 Check BitLocker Status

### Command

```powershell
manage-bde -status
```

### Purpose

Displays BitLocker encryption information for the device.

### Useful Information

You can review:

- Encryption status
- Protection status
- Encryption percentage
- Lock status
- Key protector information

### Example

```powershell
manage-bde -status
```

---

## 3.2 Check Windows Firewall Status

### Command

```powershell
netsh advfirewall show allprofiles
```

### Purpose

Displays Windows Defender Firewall status for all firewall profiles.

### Profiles

The command can show information for:

- Domain
- Private
- Public

### Example

```powershell
netsh advfirewall show allprofiles
```

---

## 3.3 Check Microsoft Defender Status

### Command

```powershell
Get-MpComputerStatus
```

### Purpose

Displays Microsoft Defender Antivirus status and health information.

### Useful Information

You can review information such as:

- Antivirus status
- Real-time protection
- Antispyware status
- Engine version
- Antivirus signature information
- Protection status

### Example

```powershell
Get-MpComputerStatus
```

---

# 4. Windows / Hybrid Troubleshooting

## 4.1 Force Group Policy Update

### Command

```powershell
gpupdate /force
```

### Purpose

Forces Group Policy processing on the device.

### Useful For

- Hybrid environment troubleshooting
- Domain Group Policy issues
- Policy refresh
- Active Directory-related device troubleshooting

### Example

```powershell
gpupdate /force
```

> **Note:** Group Policy is not an Intune policy mechanism. This command is mainly useful for domain-joined or hybrid environments.

---

## 4.2 Synchronize Windows Date and Time

### Command

```powershell
w32tm /resync
```

### Purpose

Requests Windows Time Service to resynchronize the system clock.

### Useful For

- Authentication problems
- Kerberos-related issues
- Domain authentication
- Time synchronization problems
- Microsoft 365 sign-in troubleshooting

### Example

```powershell
w32tm /resync
```

> **Note:** Administrative privileges may be required depending on the system state.

---

## 4.3 Check Basic Computer Information

### Command

```powershell
Get-ComputerInfo
```

### Purpose

Displays detailed Windows operating system and computer information.

### Useful For

- Initial device troubleshooting
- OS information
- Hardware information
- Windows configuration review
- Support documentation

### Example

```powershell
Get-ComputerInfo
```

---

# 5. Quick Reference

| # | Purpose | Command |
|---|---|---|
| 1 | Check Entra ID Join Status | `dsregcmd /status` |
| 2 | Refresh PRT | `dsregcmd /refreshprt` |
| 3 | Open Access Work or School | `Start-Process "ms-settings:workplace"` |
| 4 | Restart Intune Management Extension | `Restart-Service -Name IntuneManagementExtension` |
| 5 | Open IME Logs | `explorer.exe "C:\ProgramData\Microsoft\IntuneManagementExtension\Logs"` |
| 6 | Collect MDM Diagnostics | `mdmdiagnosticstool.exe -area DeviceEnrollment -cab "C:\MDMLogs.cab"` |
| 7 | Check BitLocker | `manage-bde -status` |
| 8 | Check Windows Firewall | `netsh advfirewall show allprofiles` |
| 9 | Check Microsoft Defender | `Get-MpComputerStatus` |
| 10 | Force Group Policy Update | `gpupdate /force` |
| 11 | Sync Date and Time | `w32tm /resync` |
| 12 | Check Computer Information | `Get-ComputerInfo` |

---

# 6. Recommended Troubleshooting Flow

When troubleshooting an Intune-managed Windows device, the following sequence can be useful:

```text
1. Check device identity
        ↓
dsregcmd /status
        ↓
2. Check / refresh PRT if required
        ↓
dsregcmd /refreshprt
        ↓
3. Check Work or School connection
        ↓
Access work or school
        ↓
4. Check Intune Management Extension
        ↓
Get-Service IntuneManagementExtension
        ↓
5. Restart IME if required
        ↓
Restart-Service IntuneManagementExtension
        ↓
6. Review IME logs
        ↓
C:\ProgramData\Microsoft\IntuneManagementExtension\Logs
        ↓
7. Collect MDM diagnostics if required
        ↓
mdmdiagnosticstool.exe
        ↓
8. Check security status
        ↓
BitLocker / Firewall / Defender
        ↓
9. Check Windows / Hybrid configuration
        ↓
GPUpdate / Time Sync / Computer Information
```

---

# 7. Important Security Considerations

Do not upload the following information to a public GitHub repository:

- Passwords
- Access tokens
- Refresh tokens
- Client secrets
- Private keys
- MDM diagnostic CAB files
- Intune logs
- Customer information
- User information
- Device-specific sensitive information

Use the repository for commands and scripts only.

---

# 8. Recommended Usage

Before running a command:

1. Understand what the command does.
2. Check whether Administrator privileges are required.
3. Test the command on a test device where possible.
4. Review the output.
5. Avoid making unnecessary configuration changes.
6. Follow your organization's change-management process for production devices.

---

# 9. Disclaimer

These commands are provided for IT administration, troubleshooting, learning, and support purposes.

Microsoft Intune, Microsoft Entra ID, Windows, and Microsoft Defender features can change over time. Always validate commands against the current Microsoft documentation before using them in production.

---

# Microsoft Documentation

- Microsoft Intune documentation:
  https://learn.microsoft.com/mem/intune/

- Microsoft Entra documentation:
  https://learn.microsoft.com/entra/

- Microsoft Graph documentation:
  https://learn.microsoft.com/graph/

- Windows PowerShell documentation:
  https://learn.microsoft.com/powershell/

- Microsoft Defender documentation:
  https://learn.microsoft.com/defender/



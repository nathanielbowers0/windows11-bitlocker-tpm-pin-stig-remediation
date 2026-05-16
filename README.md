# Windows 11 BitLocker TPM + PIN STIG Remediation

## Overview

This project focused on remediating DISA STIG finding `WN11-00-000031`, which requires Windows 11 systems to use BitLocker TPM + PIN authentication for pre-boot protection.

The objective was to:
- Identify the failed STIG using Tenable compliance auditing
- Manually remediate the policy through Local Group Policy
- Configure BitLocker TPM + PIN authentication
- Validate remediation using PowerShell
- Confirm compliance through a final Tenable rescan

---

# Technologies Used

- Tenable / Nessus
- Windows 11
- BitLocker
- PowerShell
- Local Group Policy Editor
- DISA STIGs
- Vulnerability Management
- Security Hardening

---

# Skills Demonstrated

- Vulnerability Management
- Windows Security Hardening
- STIG Compliance Remediation
- BitLocker Configuration
- Group Policy Administration
- PowerShell Validation
- Compliance Auditing
- Troubleshooting
- Security Documentation

---

# STIG Information

## STIG ID

```text
WN11-00-000031
```

## Requirement

```text
Windows 11 systems must use a BitLocker PIN for pre-boot authentication.
```

This requirement ensures systems use TPM + PIN authentication before the operating system loads, providing additional protection against unauthorized access and offline attacks.

---

# Initial Compliance Scan

An initial Tenable compliance scan was performed to identify existing STIG findings on the Windows 11 system.

The scan detected the following failed compliance finding:

```text
WN11-00-000031 - Windows 11 systems must use a BitLocker PIN for pre-boot authentication.
```

## Screenshot 1 – Initial Failed STIG Detection

<img width="1035" height="275" alt="Failed_Stig" src="https://github.com/user-attachments/assets/2efe1796-17d8-45a4-a0ae-beaf1e5dd0b8" />


---

# Manual Remediation

## Local Group Policy Configuration

The following Local Group Policy setting was opened:

```text
Computer Configuration
 → Administrative Templates
 → Windows Components
 → BitLocker Drive Encryption
 → Operating System Drives
 → Require additional authentication at startup
```

## Screenshot 2 – Policy Location Before Configuration

<img width="711" height="446" alt="Policy" src="https://github.com/user-attachments/assets/20640835-6a8e-462a-9bc9-6b3e01c23292" />


---

The policy was configured as follows:

- Enabled = Enabled
- Configure TPM startup PIN = Require startup PIN with TPM
- Allow BitLocker without a compatible TPM = Unchecked

This configuration enforces TPM + PIN authentication during system startup.

## Screenshot 3 – Policy Configured for TPM + PIN Authentication

<img width="717" height="728" alt="Configured_Policy" src="https://github.com/user-attachments/assets/0aa99190-fe86-4d0e-b289-8c9ec67a30d3" />


---

# Applying Group Policy Changes

After configuring the policy, Group Policy was updated using PowerShell.

## Command Executed

```powershell
gpupdate /force
```

The policy update completed successfully.

## Screenshot 4 – Successful Group Policy Update

<img width="716" height="407" alt="Group_Policy_Update" src="https://github.com/user-attachments/assets/9bc7d970-d178-42ff-b454-92e7543cca09" />


---

# BitLocker TPM + PIN Configuration

After the policy update, BitLocker TPM + PIN authentication was configured using PowerShell.

## Command Executed

```powershell
manage-bde -protectors -add C: -TPMAndPIN
```

During the first attempt, an error occurred because the provided PIN did not meet the minimum length requirements.

A valid PIN was then entered successfully and the TPM + PIN protector was added to the system.

## Screenshot 5 – TPM + PIN Protector Added Successfully

<img width="696" height="685" alt="TPN_PIN" src="https://github.com/user-attachments/assets/013e6a5b-b4ca-4fba-a144-c0a1a82067a9" />


---

# Validation

Validation was performed using the following command:

```powershell
manage-bde -protectors -get C:
```

The output confirmed that the system was successfully configured with:

```text
TPM And PIN
```

This verified successful implementation of BitLocker pre-boot PIN authentication.

## Screenshot 6 – Validation of TPM + PIN Configuration

<img width="714" height="680" alt="Validation" src="https://github.com/user-attachments/assets/6c976109-01b7-4d75-ad17-e1338a02960f" />


---

# Final Validation Scan

A final Tenable compliance scan was executed after configuring BitLocker TPM + PIN authentication.

## Result

```text
STIG Status: PASSED
```

The system successfully passed the compliance validation scan, confirming remediation of the DISA STIG finding.

## Screenshot 7 – Final Passed Compliance Scan

<img width="1077" height="39" alt="Passed_Scan" src="https://github.com/user-attachments/assets/2ad0dc43-adcc-4c80-8c3d-9a323bdf90b2" />


---

# Conclusion

This lab demonstrated the full vulnerability management lifecycle for a DISA STIG compliance finding involving BitLocker pre-boot authentication.

The remediation process included:
- Initial compliance auditing
- Manual Group Policy remediation
- BitLocker TPM + PIN configuration
- PowerShell validation
- Final compliance verification through Tenable

The Windows 11 system was successfully remediated and validated as compliant with DISA STIG requirement `WN11-00-000031`.

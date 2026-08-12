# Security Lab — Detection Engineering Portfolio

Home lab detection rules and DFIR investigation write-ups.

## Lab Environment
- Proxmox VE 8.4 hypervisor
- Windows Server 2022 — Active Directory (lab.local)
- Windows 10 — domain-joined workstation
- Wazuh SIEM — log ingestion and alerting

## Sigma Rules
Detection rules mapped to MITRE ATT&CK framework.

| Rule | Technique | Tactic |
|---|---|---|
| Password Spray | T1110.003 | Credential Access |
| Account Lockout | T1110.001 | Credential Access |

## Incident Reports

| Case | Platform | Tools | Date |
|---|---|---|---|
| Insider Investigation | CyberDefenders | FTK Imager | Aug 2026 |

## Author
Kent Yam — transitioning from Identity Governance into SOC and DFIR

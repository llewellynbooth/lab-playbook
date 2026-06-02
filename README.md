# Lab Playbook

Single operational playbook for the Hybrid Automation Engineering Lab.

## Lab Status
**Phase:** Week 4 -- Security baseline, domain accounts, laptop access complete
**Both Hyper-V hosts:** Fully configured
**Azure Arc:** All 5 machines Connected
**Azure Monitor:** AMA installed, data flowing
**GPO:** LAB-SecurityBaseline applied and verified
**Laptop access:** RDP and WAC working from laptop

## Contents

| Folder | Contents |
|---|---|
| playbook/ | Master playbook document |
| diagrams/ | Architecture diagrams |
| weekly-workbooks/ | Weekly workbook entries |
| incident-log/ | Unplanned incident records |
| screenshots/ | Evidence for promotion and interviews |
| docs/ | Supporting reference documents |

## Quick Links
- [Week 1 Workbook](weekly-workbooks/week-01.md)
- [Week 2 Workbook](weekly-workbooks/week-02.md)
- [Week 3 Workbook](weekly-workbooks/week-03.md)
- [Week 4 Workbook](weekly-workbooks/week-04.md)
- [Incident Log](incident-log/README.md)

## Host Status
| Host | IP | OS | Hyper-V | Arc | Status |
|---|---|---|---|---|---|
| HVHOST1 | 192.168.20.10 | WS2025 DC Eval | Yes | Connected | Ready |
| HVHOST2 | 192.168.20.11 | WS2025 DC Eval | Yes | Connected | Ready |

## VM Status
| VM | Host | IP | Role | Arc | Status |
|---|---|---|---|---|---|
| DC1 | HVHOST1 | 192.168.10.10 | AD DS, DNS | Connected | Running |
| DC2 | HVHOST2 | 192.168.10.11 | AD DS, DNS | Connected | Running |
| JUMPHOST | HVHOST2 | 192.168.10.20 | WAC, RSAT, PAW | Connected | Running |
| pfSense | HVHOST1 | 192.168.20.20 | Firewall, Router | N/A | Running |

## Azure Status
| Resource | Name | Region | Status |
|---|---|---|---|
| Resource Group | rg-lab-hybridinfra-australiaeast | Australia East | Active |
| Log Analytics | law-lab-01 | Australia East | Active |
| Data Collection Rule | dcr-lab-windows | Australia East | Active |
| Budget | hybridinfra Budget | Subscription | A$20/month |

---

## Lab Roadmap -- Full Build Sequence

### Phase 1 -- Foundation (Weeks 1-3) COMPLETE
| Week | Focus | Key Outcomes |
|---|---|---|
| 1 | Physical setup, Hyper-V, virtual switches | Both hosts running, vSwitch-External configured |
| 2 | Active Directory, DC2, pfSense, VLAN routing, Jumphost, WAC | Full AD DS, domain, VLAN routing, WAC management |
| 3 | Azure Arc, Azure Monitor, AMA, Log Analytics, cost management | Full estate Arc-enabled, data flowing to Azure |

### Phase 2 -- Identity, Security and Networking (Weeks 4-7)
| Week | Focus | Key Outcomes |
|---|---|---|
| 4 | Domain hardening, GPO, user accounts, RBAC, pfSense firewall rules | Tiered admin model, security baseline, VLAN enforcement |
| 5 | PKI and internal CA, certificate templates, auto-enrolment | Root CA, subordinate CA, WAC cert, machine certs |
| 6 | WireGuard VPN, DDNS, remote access, pfSense hardening | Remote lab access from anywhere, tested tunnel |
| 7 | Storage -- File Server, DFS, SMB permissions, Storage Spaces | File services, distributed namespace, share management |

### Phase 3 -- Automation and Cloud (Weeks 8-11)
| Week | Focus | Key Outcomes |
|---|---|---|
| 8 | GitHub repos, OIDC auth, Terraform foundations, remote state | Secretless pipelines, Azure infra as code |
| 9 | GitHub Actions CI/CD, terraform plan/apply on PR, drift detection | Full pipeline running, automated drift alerts |
| 10 | Azure Update Manager, Azure Policy, Automanage, compliance | Centralised patching, policy enforcement |
| 11 | Advanced monitoring -- KQL alerting, workbooks, dashboards | Heartbeat alerts, cost anomaly detection, dashboards |

### Phase 4 -- Linux and Cross-Platform (Week 12)
| Week | Focus | Key Outcomes |
|---|---|---|
| 12 | Linux Arc VM (Ubuntu), AMA for Linux, Syslog DCR, cross-platform monitoring | Linux in the estate, dcr-lab-linux, KQL across platforms |

### Phase 5 -- High Availability (Weeks 13-15)
| Week | Focus | Key Outcomes |
|---|---|---|
| 13 | Failover Clustering foundations, quorum design, cloud witness | Working cluster, quorum understood |
| 14 | Storage Spaces Direct, S2D design, fault domains, CSVs | S2D pool and volumes, Cluster Shared Volumes |
| 15 | Azure Local (nested) -- Arc integration, portal management | Azure Local registered, managed from Azure portal |

### Phase 6 -- Disaster Recovery and Migration (Weeks 16-17)
| Week | Focus | Key Outcomes |
|---|---|---|
| 16 | Azure Backup, Azure Site Recovery concepts, DC backup and recovery | Backup policy, tested restore, recovery runbook |
| 17 | Azure Migrate, Storage Migration Service, workload migration | Migration assessment, tested migration path |

### Phase 7 -- Advanced Security (Weeks 18-19)
| Week | Focus | Key Outcomes |
|---|---|---|
| 18 | Defender for Identity, Defender for Cloud, security posture | MDI sensors, Secure Score, recommendations actioned |
| 19 | SQL Server VM, Arc-enabled SQL, SQL monitoring, DC failure runbook | SQL in estate, tested DC failure and recovery |

### Phase 8 -- Automation Engineering (Weeks 20+)
| Week | Focus | Key Outcomes |
|---|---|---|
| 20 | PowerShell DSC, Arc Run Command, configuration as code | DSC applied to Arc VMs via pipeline |
| 21 | Terraform modules, Key Vault integration, secrets management | Reusable modules, zero secrets in code |
| 22 | Ansible for Windows, cross-platform configuration | Ansible managing Windows and Linux |
| 23 | Full lab review, exam prep, portfolio documentation | All evidence captured, certifications planned |

---

## AZ-800 / AZ-801 Coverage Map

| Exam Domain | Week(s) | Status |
|---|---|---|
| Deploy and manage AD DS | 2, 4 | Week 2 and 4 done |
| Manage AD DS and advanced identities | 4 | Week 4 done |
| Manage Windows Servers in hybrid environment | 3, 10 | Week 3 done |
| Manage virtual machines | 1 | Week 1 done |
| Implement and manage hybrid networking | 2, 6 | Partial -- Week 6 adds VPN |
| Manage storage and file services | 7 | Planned |
| Secure Windows Server infrastructure | 5, 18 | Planned |
| Implement high availability | 13, 14 | Planned |
| Implement disaster recovery | 16 | Planned |
| Migrate servers and workloads | 17 | Planned |
| Monitor and troubleshoot Windows Server | 3, 11 | Week 3 done |

---

## Certification Roadmap

| Timeline | Certification | Lab Phases That Build It |
|---|---|---|
| Month 3-4 | AZ-800 + AZ-801 (Hybrid Cloud Admin) | Phases 1-4 |
| Month 6 | HashiCorp Terraform Associate 003 | Phase 3 -- Weeks 8-9 |
| Month 12+ | AZ-400 (DevOps Engineer) | Phase 3 -- GitHub Actions, pipelines |
| Month 18+ | AZ-500 (Security Engineer) | Phase 7 -- Defender, IAM, threat model |

---

## Naming Conventions

### On-Premises
| Resource | Name |
|---|---|
| Hyper-V Host 1 | HVHOST1 |
| Hyper-V Host 2 | HVHOST2 |
| Domain Controller 1 | DC1 |
| Domain Controller 2 | DC2 |
| Jumphost / PAW | JUMPHOST |
| Arc Windows VM | ARCWIN01 |
| Arc Linux VM | ARCLIN01 |
| SQL Server VM | SQLSRV01 |
| pfSense VM | pfSense |
| AD Domain | lab.hybridinfra.dev |

### Azure Resources
| Resource | Name |
|---|---|
| Resource Group -- core | rg-lab-hybridinfra-australiaeast |
| Log Analytics Workspace | law-lab-01 |
| Key Vault | kv-lab-core-01 |
| Storage Account (TF state) | stlabterraformstate01 |
| DCR -- Windows | dcr-lab-windows |
| DCR -- Linux | dcr-lab-linux |
| Action Group | ag-lab-alerts |
| Service Principal (Terraform) | sp-lab-terraform |
| Service Principal (GitHub Actions) | sp-lab-github-actions |

### GitHub Repositories
| Repo | Purpose |
|---|---|
| lab-playbook | This document and all documentation |
| lab-infrastructure | All Terraform code and modules |
| lab-configuration | PowerShell DSC and config scripts |
| lab-pipelines | Reusable GitHub Actions workflow templates |

---

## Open Items (Current)
- JUMPHOST RAM upgrade -- 4GB to 8GB
- PKI / internal CA -- Week 5
- WAC certificate -- Week 5 (self-signed, browser shows Not secure)
- WireGuard VPN -- Week 6
- Work VS Azure benefit -- follow up with employer
- Cost Ledger -- update monthly
- ARCWIN01 and ARCLIN01 VMs -- not yet deployed
- SQL Server VM -- not yet deployed
- Azure Key Vault -- not yet deployed
- Terraform remote state -- not yet deployed
- GitHub Actions pipelines -- not yet deployed
- Audit logs in Log Analytics -- verify DCR collecting Security event log data

- [Week 5 Workbook](weekly-workbooks/week-05.md)

- [Week 5b Workbook](weekly-workbooks/week-05b.md)

- [Week 5b Workbook](weekly-workbooks/week-05b.md)

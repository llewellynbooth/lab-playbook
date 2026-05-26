# Week 5 Workbook

**Dates:** 26/05/2026
**Focus:** PKI and internal CA, certificate templates, WAC certificate binding, machine certificate auto-enrolment

## Problem Statement
Deploy an internal Certificate Authority on DC1, issue a CA-signed certificate to JUMPHOST to replace the WAC self-signed certificate, and configure Group Policy auto-enrolment so domain computers receive machine certificates automatically.

## Constraints
- JUMPHOST running Windows Server 2025 -- WAC v2.6 uses ASP.NET Kestrel, not HTTP.sys, so certificate binding works differently from all published documentation
- WAC v2 stores certificate configuration in appsettings.json rather than via netsh http add sslcert
- Laptop local admin password unavailable -- CA root cert installed to Current User store only
- Single-tier PKI only -- RAM constraints prevent a separate Root CA VM

## Success Criteria
- [x] Enterprise CA deployed on DC1 -- lab-hybridinfra-CA
- [x] Certificate template created with correct provider and permissions
- [x] CA-signed certificate issued to JUMPHOST with correct Subject and SANs
- [x] WAC running on port 443 with CA-issued certificate -- browser shows lab-hybridinfra-CA as issuer
- [x] Auto-enrolment GPO configured and applying to domain computers
- [x] JUMPHOST auto-enrolled for machine certificate via Computer-AutoEnrol template
- [x] DC1 and DC2 auto-enrolled for Domain Controller certificates

## Non-Goals
- Two-tier PKI (Root CA + Subordinate CA) -- single-tier accepted for lab
- Laptop showing trusted certificate -- local admin required for Local Machine store
- WireGuard VPN -- Week 6

## Implementation Summary

### CA Deployment
- Installed AD CS role on DC1 as Enterprise Root CA
- CA name: lab-hybridinfra-CA
- Single-tier design -- CA co-located on DC1, acceptable for lab, documented as ADR

### Certificate Template -- WAC-WebServer-CSP
- Duplicated Web Server template
- Set Provider Category to Legacy Cryptographic Service Provider (CSP) -- required for WAC
- Published template on CA
- Granted JUMPHOST$ Read, Enroll permissions

### Certificate Issuance -- WAC Certificate
- Initial attempts using Get-Certificate produced blank Subject -- known issue with how the cmdlet handles subject population
- Used certreq with INF file to produce cert with correct Subject, SANs, and CSP provider
- Final certificate: CN=JUMPHOST.lab.hybridinfra.dev, DNS=JUMPHOST.lab.hybridinfra.dev, IP=192.168.10.20
- Thumbprint: 2af418ed0a9d0f13ade9be95ca2e5ac119c41070

### WAC Certificate Binding
- WAC v2.6 installed as standalone -- does not use netsh http sslcert binding
- WAC uses Kestrel web server internally -- certificate configured via appsettings.json
- Installer GUI used to bind certificate -- selected thumbprint showing AutoConfigured status
- WAC service started successfully with CA-issued certificate
- Verified in browser -- padlock shows Issued By: lab-hybridinfra-CA

### Auto-Enrolment
- Created PKI-AutoEnrolment GPO linked to domain root
- Configured Certificate Services Client - Auto-Enrollment under both Computer and User configuration
- Published Computer-AutoEnrol template -- duplicate of built-in Computer template with schema version updated to Server 2016 compatibility and Subject name format set to Common Name
- Built-in Computer template (schema v1) returned CERTSRV_E_UNSUPPORTED_CERT_TYPE -- incompatible with this CA configuration
- Domain Computers granted Read, Enroll, Autoenroll on Computer-AutoEnrol template
- JUMPHOST auto-enrolled -- certificate visible in LocalMachine\My store
- DC1 and DC2 auto-enrolled for Domain Controller, Directory Email Replication, and Kerberos Authentication certificates

## Failures and Blast Radius

| What broke | Blast radius | Recovery |
|---|---|---|
| Get-Certificate produced blank Subject on all attempts | Certs issued but unusable -- WAC Kestrel could not find cert by Subject | Switched to certreq with INF file specifying Subject explicitly |
| WAC v2 uses Kestrel not HTTP.sys -- netsh binding does not apply | All netsh-based binding attempts failed silently | Used WAC installer GUI to bind certificate via appsettings.json |
| KSP provider cert rejected by WAC installer -- PreConfigurationRequired | WAC could not use the certificate | Created CSP-based template using Legacy Cryptographic Service Provider |
| Built-in Computer template incompatible with CA -- CERTSRV_E_UNSUPPORTED_CERT_TYPE | Auto-enrolment not issuing machine certs | Duplicated Computer template with updated schema version and correct Subject name format |
| Subject name format set to None on duplicated templates | All certs issued with blank Subject | Set Subject name format to Common Name on Computer-AutoEnrol template |
| Inbox WAC (WindowsAdminCenterSetup feature) blocking standalone installer | MSI installer returned error 1620 | Removed inbox WAC feature before installing standalone |

## Architecture Decision Records

**ADR-012 -- Single-tier PKI on DC1**
- Decision: Deploy Enterprise Root CA directly on DC1, no separate Root CA VM
- Context: RAM constraints prevent additional VM. Lab objective is PKI knowledge, not production-grade security architecture
- Consequences: Compromising DC1 also compromises CA. Documented and accepted for lab use

**ADR-013 -- Legacy CSP provider for WAC certificate**
- Decision: Certificate template configured with Legacy Cryptographic Service Provider (CSP)
- Context: WAC v2 installer shows PreConfigurationRequired for KSP certs and AutoConfigured for CSP certs
- Consequences: CSP is the older provider but fully supported and required for WAC compatibility

**ADR-014 -- certreq INF file for WAC certificate enrolment**
- Decision: Use certreq with INF file rather than Get-Certificate for WAC certificate
- Context: Get-Certificate does not reliably populate the Subject field when using Supply in request templates
- Consequences: More steps but produces a reliable, correctly-formed certificate

**ADR-015 -- Computer-AutoEnrol template replacing built-in Computer template**
- Decision: Duplicate Computer template with schema version updated to Windows Server 2016 compatibility
- Context: Built-in Computer template (schema version 1) returned CERTSRV_E_UNSUPPORTED_CERT_TYPE on this CA
- Consequences: Custom template must be maintained but gives full control over template settings

## Reflections

**What I learned:**
- KSP vs CSP provider is critical for WAC -- KSP certs show PreConfigurationRequired, CSP certs show AutoConfigured
- WAC v2 uses Kestrel internally, not HTTP.sys -- all documentation referencing netsh is written for WAC v1
- Get-Certificate does not reliably supply Subject when template is set to Supply in request -- use certreq with INF file
- Certificate template Subject name format defaults to None on duplicated templates -- must be set to Common Name
- Auto-enrolment requires Autoenroll permission in addition to Read and Enroll
- certutil -pulse must be run elevated as Administrator

**What I would do differently:**
- Start with certreq INF file for all certificate requests -- avoid Get-Certificate when Subject must be explicitly set
- Check KSP vs CSP on the template before requesting any WAC certificate
- Use WAC installer GUI from the start -- shows AutoConfigured vs PreConfigurationRequired status

## Open Items
- JUMPHOST RAM upgrade -- 4GB, deferred
- Laptop browser showing Not secure on IP -- use FQDN from domain-joined machines
- CA root cert on laptop -- Current User store only, add hosts file entry to use FQDN
- WireGuard VPN -- Week 6
- Work VS Azure benefit -- follow up with employer
- ARCWIN01 and ARCLIN01 VMs -- not yet deployed
- Audit logs flowing to Azure Monitor -- verify in Log Analytics

## Next Automation Candidate
PowerShell script to automate certificate template creation -- provider category, schema version, subject name format, and permissions in a single script.

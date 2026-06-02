# Week 5b Workbook

**Dates:** 03/06/2026
**Focus:** PKI completion — CRL/CDP, OCSP, CA backup, two-tier PKI theory, NDES and web enrollment

## Problem Statement
Complete the PKI implementation started in Week 5. Configure CRL distribution over HTTP, deploy OCSP for real-time revocation checking, document CA backup and recovery procedures, design two-tier PKI architecture, and install NDES and web enrollment roles.

## Constraints
- Single-tier PKI — CA co-located on DC1, accepted for lab
- WAC v2 Kestrel requires CSP-based certificates — KSP certs fail silently
- CA backup on DC1 only — offsite backup strategy deferred to future session
- WAC cert reissued during session due to missing OCSP URL — ADR-014 process required

## Success Criteria
- [x] IIS installed on DC1, CertEnroll virtual directory serving CRL over HTTP
- [x] CDP configured with HTTP URL and correct flags — new certs include HTTP CDP
- [x] CRL published to AD and HTTP, revocation test passed
- [x] OCSP Online Responder installed and configured — status: Working
- [x] OCSP signing cert issued via wizard — auto-enrol working
- [x] OCSP URL added to AIA — new certs include http://DC1.lab.hybridinfra.dev/ocsp
- [x] CA backup created — private key, database, registry config
- [x] Two-tier PKI documented as ADR-015
- [x] NDES installed with svc-ndes service account
- [x] Web Enrollment installed and responding over HTTPS
- [x] WAC cert reissued with correct CSP provider, SAN, and OCSP URL

## Non-Goals
- Offsite CA backup — deferred, open item
- Windows Server Backup / MABS — deferred, open item
- WAC cert IP SAN — not in original cert, HSTS cleared in browser

## Implementation Summary

### Block 1 — CRL and CDP
- Installed IIS Web Server role on DC1
- Created CertEnroll virtual directory at C:\Windows\System32\CertSrv\CertEnroll
- Enabled directory browsing and double-escaping for delta CRL filenames
- Fixed CDP flags — HTTP entry updated from flag 0 to flag 6 (ADDTOCERTCDP + ADDTOFRESHESTCRL)
- Removed dead FILE entry from CDP config
- Restarted CA service, published CRL
- Verified HTTP access: 200 OK on http://DC1.lab.hybridinfra.dev/CertEnroll/lab-hybridinfra-CA.crl
- Revocation test: revoked serial 3c0000000cb865c79e12e99e7900000000000c, confirmed in CRL

### Block 2 — OCSP Online Responder
- Installed ADCS-Online-Responder role via Install-WindowsFeature
- Published OCSPResponseSigning template to CA
- Granted DC1$ enrol permission on OCSPResponseSigning template
- Configured revocation config via MMC wizard — auto-enrol signing cert
- Root cause of initial failure: SigningFlags corrupted to 605 by failed COM object attempts
- Fix: deleted and rebuilt revocation config via wizard — flags set correctly
- OCSP signing cert issued (Request 35) and loaded — status: Working
- Added OCSP URL to AIA: http://DC1.lab.hybridinfra.dev/ocsp with flag 32
- Verified: new certs show Verified OCSP in certutil -verify -urlfetch output

### Block 3 — CA Backup
- Created C:\CABackup on DC1
- certutil -backupDB — database and logs backed up
- certutil -backupKey — private key exported to lab-hybridinfra-CA.p12 (password protected)
- reg export — CA registry config saved to CAConfig.reg
- Offsite copy to JUMPHOST failed — SMB blocked, DC1 and JUMPHOST on same subnet but firewall blocks port 445
- LESSON: backup on same machine as CA is not a backup — offsite copy is mandatory
- Open item: implement Windows Server Backup targeting DC2 share

### Recovery Procedure
1. Rebuild DC1, rejoin domain, install AD CS as Enterprise CA with name lab-hybridinfra-CA
2. certutil -restoreKey C:\CABackup\lab-hybridinfra-CA.p12
3. certutil -restoreDB C:\CABackup
4. reg import C:\CABackup\CAConfig.reg
5. net start certsvc
Note: CA name must match exactly — all issued certs reference the CA name

### Block 4 — Two-tier PKI Theory
- Documented as ADR-015
- Design exercise only, no lab work

### Block 5 — NDES and Web Enrollment
- Installed ADCS-Web-Enrollment and ADCS-Device-Enrollment roles
- Web Enrollment configured via Install-AdcsWebEnrollment
- Created svc-ndes service account, added to IIS_IUSRS domain group
- NDES configured via Install-AdcsNetworkDeviceEnrollmentService
- IIS HTTPS binding created with WebServer cert for DC1
- Verified: https://DC1.lab.hybridinfra.dev/certsrv — Windows auth prompt
- Verified: https://DC1.lab.hybridinfra.dev/certsrv/mscep/mscep.dll — NDES page

### WAC Certificate Reissue
- Original cert missing HTTP CDP and OCSP URL — issued before Block 1 and 2 work
- Reissued using certreq INF with CSP provider (ADR-014 process)
- New thumbprint: 7C0A95926311289FFDC6835C4F4404A64C486155
- SAN included: DNS Name=JUMPHOST.lab.hybridinfra.dev
- Private key permissions granted to NETWORK SERVICE
- WAC functional on https://jumphost.lab.hybridinfra.dev

## Lessons Learned
- A backup only exists if you can restore from it when the source machine is unavailable. Files on the same machine being backed up are not a backup.
- Always check CA issued certificates log first when troubleshooting enrolment failures — certutil -view shows every request, requester, and disposition
- OCSP configuration via MMC wizard sets flags correctly. COM object approach requires exact flag values and clean state — use wizard for initial setup
- WAC requires CSP-based certificates. Get-Certificate always produces KSP. Always use certreq INF with ProviderName = Microsoft RSA SChannel Cryptographic Provider
- Do not change working infrastructure without a tested rollback plan

## Open Items
- CA offsite backup — implement Windows Server Backup targeting DC2 share (Week 6 or dedicated session)
- WAC-WebServer-CSP template deleted during session — recreate or document WebServer template as replacement
- CRL validity period — default 7 days not consciously reviewed, revisit
- Role distribution — all PKI roles on DC1, production would use dedicated PKI server (ADR documented)

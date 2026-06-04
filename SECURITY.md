# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.0.x   | Yes       |
| < 1.0   | No        |

The latest release and the previous minor release are supported. Older versions will not receive security patches.

## Reporting a Vulnerability

Please do not open public issues for security vulnerabilities.

- **Email:** support [at] admindroid [dot] com with details (proof of concept, affected versions, Windows version). Replace [at] with @ and [dot] with . when sending.
- Optionally, use GitHub's [private security advisory](https://github.com/admindroid-community/PowerDroid/security/advisories/new) workflow.

We will acknowledge receipt within 3-5 business days and work with you on validation, remediation, and coordinated disclosure. If a CVE is appropriate, we will handle or coordinate it.

## Security Surface

PowerDroid is a desktop application with limited external exposure:

| Area | Scope |
|------|-------|
| **Registry** | Writes only to `HKCU` (current user). Never touches `HKLM` or system-wide keys. |
| **Clipboard** | Read-only monitoring when enabled. No clipboard data is sent externally. |
| **File System** | Reads/writes only under `%APPDATA%\PowerDroid\`. |
| **Network** | Optional update check to `api.github.com` (GitHub Releases API). No other outbound calls. |

For full details on data handling, see [TRANSPARENCY.md](TRANSPARENCY.md).

## Safe Harbor

We support responsible, good-faith security research. Avoid any actions that could harm users or data, and do not access, modify, or exfiltrate data that does not belong to you.

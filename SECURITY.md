# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 2.0.x   | Yes       |
| 1.x     | Critical fixes only |
| < 1.0   | No        |

The latest release receives security patches. Please upgrade to the current version before reporting.

## Reporting a Vulnerability

Please do not open public issues for security vulnerabilities.

- **Email:** support@admindroid.com with details (proof of concept, affected versions, Windows version)

We will acknowledge receipt within 3-5 business days and work with you on validation, remediation, and coordinated disclosure. If a CVE is appropriate, we will handle or coordinate it.

## Security Surface

PowerDroid is a local desktop application (Smart Browse + Power Clip) with limited external exposure. Everything runs on-device; no user data is collected or transmitted.

| Area | Scope |
|------|-------|
| **Registry** | Writes only to `HKCU` (current user). Never touches `HKLM` or system-wide protocol keys (`Software\Classes\http`/`https`). |
| **Clipboard** | Local monitoring only, when enabled. History is stored on-device and never sent externally; monitoring can be paused or disabled. |
| **File System** | Reads/writes only under `%APPDATA%\PowerDroid\`; read-only access to browser profile folders for detection. |
| **Network** | HTTPS-only, download-only: GitHub Releases API (update check), update installer download, browser-profile avatars, and a referenced GIF fetch. No data is ever uploaded. |
| **Process launch** | Launches browsers with a controlled process start (no shell) and validated `http`/`https` arguments only. |
| **Global input hook** | A low-level keyboard hook detects only the Power Clip trigger keys; keystrokes are never recorded or stored. |

For full data-handling details, see [TRANSPARENCY.md](TRANSPARENCY.md).

## Security Measures


### Updates & integrity
- **Verified auto-updates** — on signed builds, a downloaded update installer must pass Authenticode signature verification (matching publisher) over HTTPS before it is executed. Verification runs immediately before launch to close any tamper window.
- **Publisher anchor** — once running a signed build, the app only accepts updates signed by the same certificate.
- **Runtime installer** — the .NET Desktop Runtime the installer downloads is Microsoft-signature-verified before it runs.
- **Signed release** — the app and installer are Authenticode-signed (AdminDroid / Adminware Software Private Limited) and shipped to a locked-down `Program Files` location.

### Data protection
- **Clipboard encryption at rest** — Power Clip history is encrypted per-user with Windows DPAPI; the stored file is unreadable to other user accounts, other machines, or a copied backup.
- **Sensitive-content exclusion** — content marked by other apps (password managers; `ExcludeClipboardContentFromMonitorProcessing` / `CanIncludeInClipboardHistory`) is never captured, matching native Win+V behavior.
- **Atomic writes + backups** — data is saved via temp-file + atomic replace with rolling backups to resist corruption.
- **Safe deserialization** — all on-disk data uses `System.Text.Json` with no polymorphic type handling; there is no `BinaryFormatter`/`TypeNameHandling`.
- **Import validation** — export/import files carry a `FileType` signature and are content-validated, so an unrelated JSON file cannot pass validation and overwrite your rules.

### Isolation & injection resistance
- **No argument injection** — URLs are validated and quoted; browsers launch with `UseShellExecute=false` and an explicit argument list.
- **Path-traversal prevention** — file operations are confined to the data folder; clipboard image filenames are hashed/sanitised.
- **Registry integrity** — browser registration reads and writes the same `HKCU` hive and never modifies system-managed protocol handlers.
- **IPC hardening** — the single-instance mutex and named pipe are user-scoped/ACL-restricted; state-changing verbs (e.g. default-browser re-registration) are not honored from inter-process messages.
- **Log sanitization** — URLs are stripped of query parameters before logging to avoid leaking credentials/tokens.

### Network egress controls
- **HTTPS-only, download-only** — no outbound call uploads user data.
- **SSRF guard** — the referenced-GIF and avatar fetches enforce HTTPS and block loopback/private/internal address ranges.

### Platform
- **Minimum OS** — Windows 10 build 19041; the installer blocks older builds that the app's WinRT APIs would crash on.
- **On-device OCR** — the optional "Extract text" uses the local Windows OCR engine; nothing leaves the device.

## Safe Harbor

We support responsible, good-faith security research. Avoid any actions that could harm users or data, and do not access, modify, or exfiltrate data that does not belong to you.

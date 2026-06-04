# PowerDroid Smart Browse — Trust & Transparency

> **Version:** 1.0 | **Feature:** Smart Browse | **Publisher:** [AdminDroid](https://github.com/admindroid-community)

PowerDroid Smart Browse is a browser routing tool for Windows. It routes URLs to the right browser and profile based on the rules you define. Everything runs locally on your device — no data is collected, transmitted, or shared.

---

## What Smart Browse Does

Smart Browse intercepts URLs opened from email apps, chat applications, documents, and other sources, then routes them to the browser and profile based on your rules. For example:

- Work URLs open in your work Chrome profile
- Personal links open in Firefox
- Dev portals open in Edge with your dev account

All matching and routing happen locally on your machine.

---

## Data Collection

**We collect nothing.** Specifically:

| Category | Status |
|----------|--------|
| Usage analytics / telemetry | Not collected |
| Crash reports | Not collected |
| User accounts / sign-in | Not required |
| Cookies / fingerprinting | Not used |
| Advertising identifiers | Not used |
| Cloud sync | None |

---

## Data Storage

All data is stored locally in a single folder under your user profile:

```
%APPDATA%\PowerDroid\
  ├── settings.json        — App preferences (default browser, theme, etc.)
  ├── rules.json           — URL routing rules
  ├── urlgroups.json       — URL group definitions
  ├── profilegroups.json   — Profile group definitions
  ├── avatar-cache\        — Browser profile avatars (downloaded once)
  └── log.txt              — Application logs (local only)
```

**To completely remove all data:** Delete the `%APPDATA%\PowerDroid\` folder after uninstalling.

---

## System Resources Accessed

| Resource | Purpose | Scope |
|----------|---------|-------|
| **Windows Registry (HKCU)** | Register as a browser in Windows, detect installed browsers, manage startup entry | Current user only — never writes to HKLM |
| **File system** | Read/write settings, rules, and cached avatars | Limited to `%APPDATA%\PowerDroid\` |
| **Browser profile directories** | Detect browser profiles (name, path, avatar) | Read-only access to Chrome/Edge/Firefox/Brave/Vivaldi profile folders |
| **Process list** | Single-instance check via named mutex | User-scoped mutex to prevent duplicate launches |

---

## Network Activity

Smart Browse makes **no outbound network calls** except:

| Activity | When | Purpose | Details |
|----------|------|---------|---------|
| Profile avatar download | On first launch or when profiles change | Downloads profile pictures from Google/Microsoft accounts | HTTPS only. Cached locally in `avatar-cache\`. No data uploaded. |
| Update check | When you click "Check for Updates" in Settings | Checks GitHub Releases API for newer versions | HTTPS only. Sends no user data — just reads the latest release tag. |

**No data is ever uploaded.** Any network activity is limited to downloading information and can be reviewed in `log.txt`.

---

## Permissions & Security

### What We Do

- **Path validation** — File operations are restricted to the app's data folder to prevent path traversal attacks.
- **Log sanitization** — URLs are stripped of query parameters before logging to prevent credential leakage.
- **Process isolation** — Browser launches use controlled process start to prevent argument injection.
- **Atomic file saves** — Settings are saved safely with automatic backups to help prevent data corruption.
- **Single-instance mutex** — User-scoped to prevent cross-user interference on shared machines.

### What We Don't Do

- We don't read your browsing history
- We don't monitor which URLs you visit after routing
- We don't inject scripts or modify browser behavior
- We don't run background services (Smart Browse exits after routing unless kept in the system tray)
- We don't require administrator privileges

---

## Technology Stack

| Component | Technology | Runs Locally? |
|-----------|-----------|:---:|
| Application framework | .NET 8, WPF | Yes |
| Browser detection | Windows Registry + file system | Yes |
| URL rule matching | In-process regex/wildcard engine | Yes |
| Data storage | JSON files on disk | Yes |
| Installer | Inno Setup (per-user install, no admin required) | Yes |

---

## Third-Party Dependencies

Smart Browse uses only standard .NET libraries and Windows APIs. It does not bundle or call any third-party analytics, advertising, or tracking SDKs.

---

## Open Source

PowerDroid is developed by [AdminDroid](https://github.com/admindroid-community). The source code is available on GitHub for inspection:

- **Repository:** [github.com/admindroid-community/PowerDroid](https://github.com/admindroid-community/PowerDroid)
- **Issues:** [Report bugs or request features](https://github.com/admindroid-community/PowerDroid/issues)
- **License:** See repository for license details

---

## Future Features

PowerDroid is planned as a multi-feature Windows utility suite. Future features (such as Power Clip in v2) will have their own transparency disclosures added to this document when released. The data access, permissions, and privacy details of each feature will be documented before release.

---

## Contact

Have questions about privacy or data handling? Open an issue on [GitHub](https://github.com/admindroid-community/PowerDroid/issues) or reach out to the [AdminDroid team](https://admindroid.com/support).

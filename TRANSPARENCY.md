# PowerDroid — Trust & Transparency

> **Version:** 2.0.0 | **Features:** Smart Browse + Power Clip | **Publisher:** [AdminDroid](https://github.com/admindroid-community)

PowerDroid is a Windows productivity suite with two features: **Smart Browse** (routes URLs to the right browser and profile based on your rules) and **Power Clip** (a local clipboard history manager). Everything runs locally on your device — no data is collected, transmitted, or shared.

---

## What Smart Browse Does

Smart Browse intercepts URLs opened from email apps, chat applications, documents, and other sources, then routes them to the browser and profile based on your rules. For example:

- Work URLs open in your work Chrome profile
- Personal links open in Firefox
- Dev portals open in Edge with your dev account

Rules and groups can also open links directly in a **private/incognito window** when you choose. All matching and routing happen locally on your machine.

## What Power Clip Does

Power Clip keeps a searchable history of what you copy — text, links, file paths, images, animated GIFs, and rich text — so you can paste something again later. Everything is stored **locally**; nothing is uploaded.

- Open the history with a shortcut (double-Shift by default) and paste any past item
- Pin items you want to keep; search, multi-select, and paste in sequence
- Clipboard monitoring can be **paused or turned off** at any time, and content that apps mark as sensitive (e.g. password managers) is never captured
- Your clipboard history is stored **encrypted at rest** on your device (Windows DPAPI) — readable only by your Windows account, not by other users, other machines, or a backup copy

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
| Clipboard contents | Stored locally only (history encrypted at rest) — never uploaded |

---

## Data Storage

All data is stored locally in a single folder under your user profile:

```
%APPDATA%\PowerDroid\
  ├── settings.json            — App preferences (default browser, theme, etc.)
  ├── rules.json               — URL routing rules
  ├── urlgroups.json           — URL group definitions
  ├── profilegroups.json       — Profile group definitions
  ├── powerclip-settings.json  — Power Clip preferences (limits, capture toggles)
  ├── powerclip-history.json   — Power Clip clipboard history, encrypted at rest (Windows DPAPI)
  ├── clip-images\             — Captured clipboard images and GIFs
  ├── backups\                 — Rolling backups of your settings/rules
  ├── avatar-cache\            — Browser profile avatars (downloaded once)
  └── log.txt                  — Application logs (local only)
```

**To completely remove all data:** Delete the `%APPDATA%\PowerDroid\` folder after uninstalling (the uninstaller also removes it).

---

## System Resources Accessed

| Resource | Purpose | Scope |
|----------|---------|-------|
| **Windows Registry (HKCU)** | Register as a browser in Windows, detect installed browsers, manage startup entry | Current user only — never writes to HKLM |
| **File system** | Read/write settings, rules, clipboard history, and cached images/avatars | Limited to `%APPDATA%\PowerDroid\` |
| **Browser profile directories** | Detect browser profiles (name, path, avatar) | Read-only access to Chrome/Edge/Firefox/Brave/Opera profile folders |
| **Windows clipboard** | Power Clip: capture what you copy into local history | Event-driven; honours "exclude from history" markers; can be paused/disabled |
| **Global keyboard shortcut** | Open the Power Clip popup and other configured shortcuts | Low-level hook inspects only modifier/hotkey keys — keystrokes are never recorded or stored |
| **On-device text recognition (OCR)** | Optional "Extract text" from a copied image | Uses the built-in Windows OCR engine locally; nothing leaves the device |
| **Process list** | Single-instance check; detect the app a clip came from | User-scoped mutex; foreground-app name only |

---

## Network Activity

PowerDroid makes **no outbound network calls** except:

| Activity | When | Purpose | Details |
|----------|------|---------|---------|
| Profile avatar download | On first launch or when profiles change | Downloads profile pictures from Google/Microsoft accounts | HTTPS only; blocks local/private/internal network addresses. Cached locally. No data uploaded. |
| Update check | When you click "Check for Updates" (and a twice-daily background check) | Checks the GitHub Releases API for newer versions | HTTPS only. Sends no user data — just reads the latest release info. |
| Update download | When you choose to install an available update | Downloads the installer, then verifies it before running | HTTPS only. On signed builds the installer's Authenticode signature is verified before it runs. |
| Animated-GIF fetch | When you copy rich content that references a GIF by URL | Fetches the GIF so it can be shown/pasted from history | HTTPS only; blocks local/private network addresses. No data uploaded. |

**No data is ever uploaded.** All network activity is limited to downloading and can be reviewed in `log.txt`.

---

## Permissions & Security

### What We Do

- **Clipboard privacy** — Content that applications mark as sensitive (e.g. password managers, "exclude from clipboard history") is never captured. Monitoring can be paused or fully disabled, with a clear indicator when it is off.
- **Encrypted clipboard history** — Power Clip history is encrypted at rest using Windows DPAPI (per-user). The stored file is an unreadable blob to other user accounts, other machines, or a backup copy; existing history is migrated to the encrypted format automatically.
- **Path validation** — File operations are restricted to the app's data folder; clipboard image filenames are hashed/sanitised to prevent path traversal.
- **Log sanitization** — URLs are stripped of query parameters before logging to prevent credential leakage.
- **Process isolation** — Browser launches use controlled process start (no shell) and validate the target so only genuine `http`/`https` URLs are passed to the browser, preventing command/argument injection; only such URLs and existing local files/folders can be opened from history.
- **Verified updates** — On signed builds, downloaded updates must pass Authenticode signature verification (matching publisher) over HTTPS before they run. The installer likewise verifies the **Microsoft signature** of the .NET runtime it downloads before running it.
- **Atomic file saves** — Data is saved safely with automatic backups to help prevent corruption.
- **Single-instance mutex** — User-scoped to prevent cross-user interference on shared machines.

### What We Don't Do

- We don't read your browsing history
- We don't monitor which URLs you visit after routing
- We don't inject scripts or modify browser behavior
- We don't record keystrokes (the global shortcut hook only detects its trigger keys)
- We don't upload your clipboard contents anywhere — Power Clip history stays on your device
- The app itself needs no administrator privileges to run

> **Background operation:** With Power Clip and global shortcuts enabled, PowerDroid stays resident in the system tray to watch the clipboard and serve shortcuts. You can quit it from the tray menu at any time; Smart Browse alone does not need to stay resident.

---

## Technology Stack

| Component | Technology | Runs Locally? |
|-----------|-----------|:---:|
| Application framework | .NET 8, WPF | Yes |
| Browser detection | Windows Registry + file system | Yes |
| URL rule matching | In-process regex/wildcard engine | Yes |
| Clipboard history | Local DPAPI-encrypted history file + image files on disk | Yes |
| Firefox profile names | Embedded SQLite (bundled, local) | Yes |
| Text recognition (OCR) | Windows built-in OCR engine | Yes |
| Data storage | JSON files on disk | Yes |
| Installer | Inno Setup (installs to Program Files; requires administrator) | Yes |

---

## Third-Party Dependencies

PowerDroid uses standard .NET libraries, Windows APIs, and a small set of open components (a system-tray helper and an embedded SQLite engine). It does not bundle or call any third-party analytics, advertising, or tracking SDKs.

---

## Open Source

PowerDroid is developed by [AdminDroid](https://github.com/admindroid-community). The source code is available on GitHub for inspection:

- **Repository:** [github.com/admindroid-community/PowerDroid](https://github.com/admindroid-community/PowerDroid)
- **Issues:** [Report bugs or request features](https://github.com/admindroid-community/PowerDroid/issues)
- **License:** See repository for license details

---

## Contact

Have questions about privacy or data handling? Open an issue on [GitHub](https://github.com/admindroid-community/PowerDroid/issues) or reach out to the [AdminDroid team](https://admindroid.com/support).

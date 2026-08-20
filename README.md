# PowerDroid — Smart Browse & Power Clip for Windows

> **Version 2.0** · Two features, one lightweight app · by [AdminDroid](https://admindroid.com)

PowerDroid is a Windows productivity suite with two features that work great together:

- **🌐 Smart Browse** — automatically opens every link in the right browser and profile based on rules you create.
- **📋 Power Clip** — a fast, private clipboard history manager for text, links, images, and more.

Everything runs **locally** on your device — no accounts, no tracking, no cloud, and no internet connection required to use it.

👉 Explore PowerDroid: [http://admindroid.com/powerdroid-smart-browse](http://admindroid.com/powerdroid)


---

# 🌐 Smart Browse — Automatic Link Routing

![PowerDroid Smart Browse Tool](https://blog.admindroid.com/wp-content/uploads/2026/06/PowerDroid-Smart-Browse-Tool.png)

Smart Browse helps you automatically open every link in the correct browser and profile based on routing rules you create.

Instead of manually switching browsers, copying links, or selecting profiles every time, Smart Browse intelligently routes URLs across work, personal, admin, and development workflows — perfect for anyone managing multiple browsers, profiles, environments, and admin portals.

**Automatic Routing · URL Groups · Clipboard URL Detection · Rule Management · Unmatched URL Alerts**

## Why Smart Browse?

Windows always opens links with a single default browser, which creates constant browser switching and profile conflicts:

- Work links opening in personal profiles
- Admin portals opening in the personal browser
- Repeated copy-paste workflows
- Constant manual browser switching

Smart Browse solves this by automatically routing links based on rules you configure once.

## How It Works

1. **Set PowerDroid Smart Browse as your default browser** — every link you click from Teams, Outlook, Slack, and other desktop apps is intercepted automatically.
2. **Create routing rules** — assign URL patterns and domain groups to specific browsers and profiles.
3. **Smart Browse matches every link** — it instantly checks your saved rules whenever you open or copy a URL.
4. **Links open in the right browser and profile** — automatically, with no manual switching.

## Core Features

- **URL Groups for related domains** — group related domains into a single routing rule with one browser profile. Built-in templates included for **Microsoft 365** and **Google Workspace**.
- **Clipboard URL detection** — watches the clipboard for copied URLs and, when one matches a rule, shows a pop-up to open it in the right browser/profile in one click. Can be paused (5/15/30 min or 1 hour) or disabled.
- **Create rules instantly from notifications** — when a link matches no rule, a toast lets you create a routing rule on the spot.
- **Rules management** — enable/disable, search, sort, edit, delete, and move rules between grouped and individual modes.
- **Automatic conflict detection** — overlapping or conflicting rules are flagged before they cause surprises, with keep-or-replace options.
- **Longest-match priority routing** — when multiple rules match, the most specific one wins (e.g. `admin.microsoft.com` overrides `microsoft.com`).
- **Export & restore** — back up and restore your complete configuration (routing rules, URL groups, browser settings) anytime.

## New in Version 2.0

- **Incognito / private-window routing** — open a rule or group's links directly in a private/incognito window.
- **Rule descriptions** and a **full-URL ↔ domain toggle** when creating rules from a copied link.
- **Targetable fallback profile** — pick the exact default-browser profile (with an avatar picker) used when no rule matches.
- **Real Firefox profile names** — friendly profile names instead of internal folder names.
- **First-run welcome overlay** and a **one-click "Open Windows Settings"** button to set PowerDroid as default.
- **Profile picker for copied URLs** that match multi-profile rules, plus browser icons on profile cards.

---

# 📋 Power Clip — Clipboard History Manager

Power Clip keeps a searchable history of everything you copy — so you can paste something again later without re-copying. Everything is stored **locally and encrypted**; nothing is ever uploaded.

## Features

- **Captures everything you copy** — text, links, file paths, images, animated GIFs, and rich text (hyperlinks and formatting preserved).
- **Fast compact popup** — open with a double-Shift trigger or a custom shortcut. It's non-activating, so it never steals focus, and supports full keyboard navigation.
- **Pin what matters** — pin up to 10 items; pinned items are kept when you clear history.
- **Search, multi-select & sequential paste** — instant filter-aware search, select multiple items, and paste them in order; selectable preview text with "Paste selection".
- **Built-in Browsers tab** — launch any browser profile from the popup, mark favourites, and open in a private/incognito window.
- **Extract text from images (OCR)** — pull text out of a copied image using the built-in Windows on-device OCR engine.
- **Image storage management** — a storage-aware usage banner and a "Free up space" action; history cap defaults to 200 items.
- **Pause or disable anytime** — clear "paused / disabled" indicators across every surface and the tray, with a "Resumes in Xm" countdown.

## Private by Design

- **Stored locally and encrypted at rest** — clipboard history is encrypted with Windows' built-in per-user encryption (DPAPI); the file is readable only by your Windows account, not by other users, other machines, or a backup copy.
- **Secrets are never captured** — content that apps mark as sensitive (password managers, banking apps, "exclude from clipboard history") is skipped entirely, using the same markers Windows Clipboard History honours.
- **Nothing is ever uploaded.**

## Works over elevated (admin) windows — install required

Power Clip's global shortcut and paste keep working even when an **elevated / administrator** window is in the foreground (an admin PowerShell or Command Prompt, Task Manager, Registry Editor, an elevated editor, etc.).

Windows only lets a program send input to, and read the clipboard for, higher-integrity (elevated) windows if that program has **UIAccess** — a privilege Windows grants **only** to an app that is **Authenticode-signed** *and* running from a **secure location** (`Program Files` or `System32`).

That's why PowerDroid must be **installed with the official signed installer** rather than run as a loose/portable `.exe`:

- The installer places the **signed `PowerDroid.exe` in `Program Files`** and enables UIAccess, so the clipboard popup, hotkeys, and paste work across **every** window — elevated or not.
- The install directory is fixed to `Program Files` for this reason (you can't relocate it); a signed exe outside a secure location would fail to launch under UIAccess.
- The app still **runs at your normal (non-elevated) user level** — UIAccess only lets it interact with elevated windows; it does not run PowerDroid itself as administrator.

> If you run an **unsigned build** or a **copied-out exe**, PowerDroid still works normally over regular windows, but its global shortcut and paste will **not** function while an elevated window is focused.

---

# 🔒 Privacy & Security

PowerDroid is built to keep your data on your device.

- **Runs entirely locally** — no ads, no tracking, no cloud dependency, no account, and no internet connection required to use it.
- **Encrypted clipboard history** (Windows DPAPI) and secret-marker exclusion so password-manager content is never stored.
- **Safe link launching** — only genuine `http`/`https` URLs are passed to the browser, preventing command/argument injection.
- **Verified auto-updates** — on signed builds, updates are downloaded over HTTPS and pass Authenticode signature verification before running; the installer also verifies the Microsoft signature of the .NET runtime it downloads.
- **No administrator rights needed** to run the app; data stays in your own user profile.

For a full breakdown of data storage, network activity, and permissions, see [TRANSPARENCY.md](TRANSPARENCY.md).

---

# 🧩 Broad Browser Support

Smart Browse and Power Clip work with popular browsers, making it easy to route URLs and launch profiles across work, personal, and development environments:

- Google Chrome
- Microsoft Edge
- Mozilla Firefox
- Brave Browser
- Opera & Opera GX

---

# 🎥 See PowerDroid in Action

Watch how Smart Browse automatically routes links to the right browser and profile across your daily workflows.

👉 Tool walkthrough demo video: [Watch on LinkedIn]()

---

# 🚀 Get PowerDroid

- Open every link in the right browser and profile — automatically
- Keep a fast, private, encrypted clipboard history
- Organize work, personal, and development workflows with ease

👉 [Download PowerDroid](http://admindroid.com/powerdroid-smart-browse)

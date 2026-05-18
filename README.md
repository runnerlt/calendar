# Family Calendar

A personal family calendar hosted on GitHub Pages. PIN-protected, AES-256-GCM encrypted, installable as a PWA on Android.

## Features

- Monthly and list calendar views (FullCalendar.js)
- Custody week schedule with alternating color tint
- Vacations and events with color coding by type
- Event descriptions shown as subtitles on calendar tiles
- Data saved as encrypted JSON to this repo via GitHub API
- PIN gate — calendar is locked behind a PIN on every visit
- Works offline after first load (PWA)

## Event Types

| Type | Color |
|---|---|
| Custody week | Green |
| My vacation | Blue |
| Daughter vacation | Orange |
| Other event | Purple |

## First-Time Setup

1. Open `https://YOUR-USERNAME.github.io/calendar/#init`
2. Set a PIN (minimum 4 characters)
3. Open the ⚙ Settings menu and enter:
   - **GitHub Username**
   - **Repository name** (e.g. `calendar`)
   - **Personal Access Token** — fine-grained PAT with **Contents: Read and Write** on this repo
4. Click **Save**

The setup URL (`#init`) is only required once. After a PIN is set, that URL is no longer needed for normal use.

> If you need to set up on a new device, open Settings → copy the **New device setup link** and open it in the new browser.

## Daily Use

- Visit `https://YOUR-USERNAME.github.io/calendar/`
- Enter PIN to unlock
- Click any date to add an event
- Click an existing event to edit or delete
- Changes are saved automatically to `events.json` in this repo

## Data Storage

All event data is stored in `events.json` in this repository, encrypted with AES-256-GCM. The encryption key is derived from your PIN using PBKDF2 (100 000 iterations, SHA-256). Without the correct PIN, the data cannot be decrypted.

`events.json` format after first save:
```json
{
  "encrypted": true,
  "salt": "<hex>",
  "iv": "<hex>",
  "data": "<base64 ciphertext>"
}
```

## Install on Android

Open the site in Chrome → tap the browser menu → **Add to Home Screen**. The app runs in standalone mode (no browser chrome).

## Changing Your PIN

Settings (⚙) → **Change PIN** → enter new PIN. All data is re-encrypted with the new key and saved automatically.

## Resetting Everything

To wipe all data and start fresh:
1. Replace `events.json` with `[]`
2. Clear `localStorage` in browser DevTools (Application → Local Storage)
3. Visit `#init` to set a new PIN

## Personal Access Token

Create at: **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens**

Required permissions:
- Repository: this repo only
- Contents: **Read and Write**

Store the token only in the app's Settings modal (saved to `localStorage`, never committed to the repo).

## Tech Stack

- [FullCalendar.js](https://fullcalendar.io/) v6 (CDN)
- Web Crypto API (built-in, no external crypto library)
- GitHub Contents API
- Pure HTML/CSS/JS — no build step, no framework

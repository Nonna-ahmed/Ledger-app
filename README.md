# The Ledger — PWA package

## What's inside
- `index.html` — the app (storage: localStorage for settings, IndexedDB for records/receipts)
- `manifest.json` — PWA manifest (name, icons, colors)
- `service-worker.js` — offline cache-first strategy + notification plumbing
- `icons/` — app icons (192, 512, 512 maskable, 180 Apple touch, 32 favicon)

## To make install/offline/notifications actually work
Browsers only allow service workers, install prompts, and push notifications on
pages served over **HTTPS** from a real domain (or `http://localhost` for testing).
Opening `index.html` directly as a file (`file://...`) will NOT enable these features.

Fastest free hosting options:
1. **GitHub Pages** — push this folder to a repo, enable Pages in repo settings.
2. **Netlify Drop** — go to app.netlify.com/drop and drag this folder in. Instant HTTPS URL.
3. **Vercel** — `vercel` CLI or drag-and-drop via vercel.com/new.
4. **Local test only**: `python3 -m http.server 8080` from this folder, open
   `http://localhost:8080` — service worker + install prompt work on localhost.

Once hosted, open the URL on your phone/desktop:
- **Android/Desktop Chrome/Edge**: an "Install" button appears automatically (or use
  the browser's install icon in the address bar).
- **iPhone (Safari)**: tap Share → "Add to Home Screen" (no install prompt API exists
  on iOS — the app shows a one-time banner reminding you of this).

## About "push notifications"
True push (a server waking your phone while the app is fully closed) needs a backend
with push credentials (VAPID keys) — that's server infrastructure, not something a
static file bundle can do alone. What's implemented here are **local notifications**:
real payday / overspending / savings-goal / recurring-bill alerts, fired through the
same Notification API and service worker, that work whenever the app is open or was
recently active. The service worker already has a `push` event handler ready, so a
backend could be wired in later without touching the front end.

## Data & language
- All data (expenses, incomes, goals, recurring bills, receipts) lives in IndexedDB
  in the browser — nothing is sent anywhere.
- Settings (salary, payday, notification toggle, language) live in localStorage.
- Language switch (EN/AR) is in the top-right of the app and is remembered.

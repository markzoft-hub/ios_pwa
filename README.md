# Roll a Task 🎲

A tiny dice-driven to-do app, inspired by a paper "roll a d6 to pick a task" notepad. Add up to six tasks, roll a die to pick which one to do next, roll a second die to pick how long you'll spend on it, then start the timer.

Installable as a home-screen app on iOS and Android (PWA), with tasks synced across your devices via Firebase.

## Features

- **Add up to 6 tasks** — one per numbered slot
- **Roll to pick a task** — only lands on slots that are filled in and not checked off
- **Roll to pick a duration** — 10 / 20 / 30 / 40 / 50 minutes, or a break
- **Selectable durations** — tap a duration to mark it unavailable (e.g. if you only have 30 minutes free), so the die only rolls against durations you actually have time for
- **Built-in countdown timer** for the rolled duration
- **Cloud sync** — changes made on one device (add, check off, remove) automatically push to the cloud and pull on the other, no login needed
- **Installable as an app** — Add to Home Screen on iOS, or "Install app" on Android/Chrome, with its own dice icon

## Setup

This app uses [Firebase Realtime Database](https://firebase.google.com/docs/database) as a lightweight, no-login sync backend, accessed directly via its REST API (no SDK required).

1. Create a free project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable **Realtime Database** (Build → Realtime Database → Create Database → test mode)
3. Under the **Rules** tab, set:
   ```json
   { "rules": { ".read": true, ".write": true } }
   ```
4. Copy your database URL (looks like `https://your-project-default-rtdb.firebaseio.com/`)
5. In `index.html`, find this line and replace it with your own URL:
   ```js
   const FIREBASE_URL = 'https://your-project-default-rtdb.firebaseio.com/';
   ```

> ⚠️ **Note on privacy:** the rules above make the database openly readable and writable by anyone who has the URL. That's fine for a personal task list shared across your own devices, but don't use this setup for sensitive data.

## Deploying

This is a single static HTML file, so it works with [GitHub Pages](https://pages.github.com/) out of the box:

1. Push `index.html` to your repo's default branch
2. Go to **Settings → Pages**, set Source to "Deploy from a branch," pick `main` and `/ (root)`
3. Your app will be live at `https://<your-username>.github.io/<repo-name>/`

## Installing on your phone

**iOS (Safari):** open the live URL → Share → Add to Home Screen
**Android (Chrome):** open the live URL → menu → Install app / Add to Home Screen

Once installed, it opens full-screen with its own icon, and both devices stay in sync automatically.

## Making changes

Edit `index.html` directly (either locally or via GitHub's web editor), commit, and GitHub Pages redeploys automatically within a minute or so. If you've already installed the app on your phone, you may need to remove and re-add the home-screen icon (or fully close and reopen it) to see certain updates, since iOS/Android cache installed web apps fairly aggressively.

## Tech notes

- Single-file HTML/CSS/JS, no build step, no dependencies
- Tasks are cached in `localStorage` so the app works offline and loads instantly, then syncs with the cloud copy in the background
- The dice icon is embedded directly in the HTML as a base64 data URI (used for the favicon, Apple touch icon, and web app manifest), so there are no external image files to manage

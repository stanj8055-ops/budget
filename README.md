# 💰 Personal Budget Dashboard — PWA Setup Guide

A Progressive Web App (PWA) that works on Android, desktop, and any browser — with real-time sync via Firebase.

---

## 🚀 Step 1 — Set up Firebase (5 minutes)

### 1a. Create a Firebase project
1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Add project"**
3. Name it `budget-dashboard` (or anything you like)
4. Disable Google Analytics (not needed) → click **Create project**

### 1b. Enable Firestore
1. In the left sidebar click **"Build" → "Firestore Database"**
2. Click **"Create database"**
3. Choose **"Start in test mode"** (we'll secure it later)
4. Pick any region close to you (e.g. `asia-southeast1` for Australia) → click **Enable**

### 1c. Get your Firebase config
1. Click the ⚙️ gear icon → **"Project settings"**
2. Scroll down to **"Your apps"** → click the **</>** (Web) icon
3. Register app name as `budget-pwa` → click **Register app**
4. You'll see a config block like this — **copy it**:
```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

### 1d. Paste config into index.html
1. Open `index.html` in any text editor (Notepad, VS Code, etc.)
2. Find the section that says:
```js
// ── YOUR FIREBASE CONFIG (fill these in after creating your Firebase project) ──
const firebaseConfig = {
  apiKey:            "PASTE_YOUR_API_KEY",
  ...
```
3. Replace the placeholder values with your actual config values
4. Save the file

---

## 🌐 Step 2 — Deploy to GitHub Pages (3 minutes)

### 2a. Create a GitHub account (if you don't have one)
Go to [https://github.com](https://github.com) → Sign up (it's free)

### 2b. Create a new repository
1. Click the **+** icon → **"New repository"**
2. Name it `budget-dashboard`
3. Set it to **Public** (required for free GitHub Pages)
4. Click **"Create repository"**

### 2c. Upload files
1. On the repository page, click **"uploading an existing file"**
2. Drag and drop ALL these files/folders:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icons/` folder (with both PNG files inside)
3. Click **"Commit changes"**

### 2d. Enable GitHub Pages
1. Go to repository **Settings** → **Pages** (left sidebar)
2. Under "Source" select **"Deploy from a branch"**
3. Branch: **main**, Folder: **/ (root)** → click **Save**
4. Wait ~2 minutes, then your app is live at:
   **`https://YOUR-USERNAME.github.io/budget-dashboard`**

---

## 📱 Step 3 — Install on Android (30 seconds)

1. Open Chrome on your Android phone
2. Go to your GitHub Pages URL
3. Tap the **three-dot menu (⋮)** → **"Add to Home screen"**
4. Tap **"Add"** — the app icon will appear on your home screen
5. Open it — it works like a native app, full screen, no browser bar!

---

## 🔄 How Sync Works

| Action | What happens |
|--------|-------------|
| Open app | Loads latest data from Firebase |
| Hit "Apply Changes" | Saves all data to Firebase |
| Fire a Debt Attack | Saves automatically |
| Another device opens app | Gets latest data within seconds |
| No internet | Works offline, syncs when reconnected |

The **sync pill** in the top-right corner shows:
- 🟢 **Synced** — all changes saved
- 🟡 **Saving…** — upload in progress  
- ⚫ **Offline** — no connection (app still works)
- 🔴 **Sync error** — check Firebase config

---

## 🔒 Step 4 (Optional) — Secure your Firestore

By default Firestore is in "test mode" (anyone can read/write for 30 days). To secure it:

1. In Firebase console → **Firestore → Rules**
2. Replace the rules with:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /budget/{document=**} {
      allow read, write: if true; // Personal use — no auth needed
    }
  }
}
```
3. Click **Publish** — this keeps it open but only to anyone with your URL

---

## 📁 File Structure

```
budget-dashboard/
├── index.html       ← Main app (edit Firebase config here)
├── manifest.json    ← PWA install config
├── sw.js            ← Service worker (offline support)
└── icons/
    ├── icon-192.png ← App icon (small)
    └── icon-512.png ← App icon (large)
```

---

## ❓ Troubleshooting

**App won't install on Android**
- Must be served over HTTPS — GitHub Pages does this automatically ✅
- Make sure `manifest.json` is in the same folder as `index.html`

**Sync not working**
- Double-check your Firebase config values in `index.html`
- Make sure Firestore is in "test mode" or rules allow writes
- Check browser console for errors (F12 → Console)

**App shows old data after update**
- Service worker caches the app — hard refresh: hold Shift + click reload
- Or clear site data: Chrome → Settings → Site settings → budget URL → Clear data

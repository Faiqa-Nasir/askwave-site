# AskWave landing page

A single self-contained `index.html` (no build step, no dependencies) — a
download page for the AskWave Android app. Two things to do: **host the page**
and **build + link the APK**.

---

## 1. Build the Android APK (the file people download)

From `askwave-frontend/`:

```bash
npm install -g eas-cli          # once
eas login                       # your Expo account
eas build -p android --profile preview
```

- The `preview` profile produces an **installable APK** (the `production`
  profile makes an `.aab` for the Play Store, which can't be sideloaded — use
  `preview`). If `eas.json` has no `preview` profile, add:

  ```json
  { "build": { "preview": { "android": { "buildType": "apk" } } } }
  ```

- When the build finishes, EAS prints a URL to download the `.apk`.
- **IMPORTANT:** the app must point at the deployed backend, not your laptop.
  Confirm `askwave-frontend/.env` (or the build profile env) has
  `EXPO_PUBLIC_API_URL=https://askwave-c9cy.onrender.com` before building —
  baked in at build time.

### Host the APK so the link is permanent
Easiest free option — **GitHub Releases**:
1. GitHub repo → **Releases** → **Draft a new release**.
2. Tag e.g. `v0.1.0`, attach the `.apk` file.
3. Publish → right-click the uploaded `.apk` → copy its download URL.
4. Paste that URL into `index.html` where it says `href="#" id="download"`.

(Re-uploading a new APK = new release; just update the link.)

---

## 2. Host the landing page for free

Pick any one — all give you a public URL:

### Option A — GitHub Pages (simplest, no account beyond GitHub)
1. Push this `askwave-landing/` folder to a repo (or a `gh-pages` branch).
2. Repo → **Settings → Pages** → Source = your branch, folder = `/askwave-landing`
   (or root if it's its own repo).
3. URL: `https://<user>.github.io/<repo>/`.

### Option B — Netlify / Vercel / Cloudflare Pages (drag-and-drop)
1. Sign in (free) at netlify.com / vercel.com / pages.cloudflare.com.
2. **Drag the `askwave-landing` folder** onto the dashboard (Netlify Drop is
   literally drag-and-drop), or "Import" the repo.
3. You get a URL like `https://askwave.netlify.app` — that's your shareable link.
   You can add a custom domain later for free.

---

## Order of operations
1. Build the APK → host on GitHub Releases → copy its URL.
2. Paste that URL into `index.html` (`id="download"` link).
3. Deploy the page (Option A or B).
4. Share the page URL. People tap **Download for Android** → install.

## Notes
- **iOS**: Apple blocks direct `.ipa` downloads. iPhone users need TestFlight
  (free, but requires an Apple Developer account, $99/yr) or the App Store. The
  page already shows an iOS note instead of a broken button.
- Edit copy, emoji, and colors directly in `index.html` — it's all inline.
- Add screenshots: drop image files in this folder and reference them with
  `<img src="screenshot.png">`.

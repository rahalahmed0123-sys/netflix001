# Netflixy Mobile

Netflix WebView app — fetches your session cookies from your own API and injects them natively so Netflix loads fully authenticated.

---

## Get the APK (GitHub Actions — no Android Studio, no Expo account needed)

### 1. Push this repo to GitHub
```bash
git init
git add .
git commit -m "Netflixy mobile"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

**Important:** If GitHub shows a suggestion to add an "Expo" or "EAS" workflow, decline it / delete it if added. This project only needs the `build-android.yml` workflow already included here — it does NOT use EAS and does NOT require an Expo account.

### 2. Watch the build
Go to your repo → **Actions** tab → "Build Android APK" starts automatically.
Takes ~10–15 minutes.

### 3. Download the APK
Click the finished run → scroll to **Artifacts** → download **netflixy-release-apk** → extract → you get `app-release.apk`.

### 4. Install on Android
- Send the APK to your phone (email, Google Drive, USB, etc.)
- Open it → allow "Install from unknown sources" when prompted → install

**Note:** This is a release build with the JS bundle embedded directly in the APK, so it works standalone with no dev server. If you ever see the app stuck on a black splash screen forever, that means a debug build was installed instead — rebuild using this workflow's `assembleRelease` step.

---

## How it works
1. Open the app → paste your Netflixy API URL
2. Tap **Connect** → fetches your Netflix cookies from `/api/access`
3. Tap **Watch Netflix** → injects ALL cookies natively into Android's WebView cookie store (including HttpOnly ones like `NetflixId`) → Netflix loads authenticated

## API format expected
`GET https://your-api.com/api/access` must return:
```json
{ "found": true, "cookieValue": "# Netscape HTTP Cookie File\n..." }
```
Cookies should be in Netscape tab-separated format. Lines starting with `#HttpOnly_` are handled correctly.

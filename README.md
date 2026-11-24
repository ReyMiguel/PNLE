# PNLE Study Hub — Android APK + Windows EXE

Unofficial self-study app for the Philippine Nurse Licensure Examination (practice, timed mock exam, reviewer, diagrams,
return-demonstration steps from photos, case log). **Not affiliated with or endorsed by the PRC. Scores are self-study only.**

The whole app is one file: `www/index.html`. This repo wraps it as:
- an **Android app** (Capacitor) → `.apk`
- a **Windows app** (Electron) → installer `.exe` and portable `.exe`

GitHub Actions builds both for you and attaches them to a GitHub Release. You do not need Android Studio or Windows.

## Release in 5 steps

1. Create a new empty repository on GitHub (private or public).
2. Upload this project. The hidden `.github` folder must be included, so using git is the safest way:
   ```bash
   cd pnle-study-hub
   git init && git add -A && git commit -m "PNLE Study Hub v1.0.0"
   git branch -M main
   git remote add origin https://github.com/<your-user>/<your-repo>.git
   git push -u origin main
   ```
   (If you use the web uploader, make sure `.github/workflows/release.yml` ends up in the repo.)
3. Create a version tag to trigger the build and release:
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```
4. Open the repo → **Actions** and wait about 10–15 minutes for *Build Android APK and Windows EXE* to finish.
5. Open **Releases**: the APK, `PNLE-Study-Hub-Setup-1.0.0.exe` and `PNLE-Study-Hub-Portable-1.0.0.exe` are attached.

To test without releasing, run the workflow manually (Actions → *Run workflow*); files appear under the run's **Artifacts**.

## Notes

- **Android:** the APK is *debug-signed*, which is fine for sideloading (turn on "Install unknown apps"). Publishing to Google Play requires a
  release keystore and an `.aab`; that is not set up here.
- **Windows:** the exe is not code-signed, so SmartScreen may warn ("More info" → "Run anyway").
- **Data:** notes, case log, progress, step sets and photos stay on that device inside the app (IndexedDB/localStorage). They don't sync.
  Use Settings → Export backup. In the Android app, exports open the share sheet so you can save the file.
- **Photo reading (OCR):** needs internet the first time (Tesseract.js is loaded from jsDelivr). Photos are never uploaded. Typing steps manually works offline.
- **Updating the app:** replace `www/index.html`, commit, and push a new tag (e.g. `v1.0.1`).
- **Not tested by the author:** these build files were prepared without being able to run the Android or Windows builds. If a job fails, open
  the failed step in the Actions log and share the error to get it fixed.

## Build locally (optional)
```bash
npm install
npx electron .                 # run the desktop app
npm run dist:win               # build Windows exes (run on Windows)
npx cap add android && npx cap sync android && npx cap open android   # Android Studio
```

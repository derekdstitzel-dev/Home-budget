# HomeLedger Android — starter project

## Current state
This is a **local-only Android wrapper** around the tested HomeLedger v2 HTML interface. It is not a compiled APK. Data is stored in WebView localStorage on one device, **not synchronized**, and is lost if app data is cleared or the app is uninstalled. The app uses Android's FLAG_SECURE to limit screenshots. The HTML's CSV import works via Android's file chooser; export/download behavior may require further native integration and must be tested before storing important records. The app has not been compiled or tested in Android Studio.

## Build
1. Install Android Studio with Android SDK 35 and JDK 17.
2. Open this directory as a Gradle project; let Android Studio sync.
3. Build > Build Bundle(s) / APK(s) > Build APK(s).
4. Install the generated debug APK on a test phone. Use synthetic data first.

## Shared family edition (not yet implemented)
- Use separate authentication for each spouse (e.g. Firebase Auth or Supabase Auth), with optional biometrics.
- Database tables: households, memberships, accounts, transactions, category_rules, budgets, transaction_reviews, audit_log.
- Enforce household isolation on the server with row-level security, not just UI filters. Both members have equal permissions, and each write records actor ID and timestamp.
- Use unique bank transaction identifiers to deduplicate imports; keep pending vs posted transactions distinct.
- Use a vetted financial-data aggregation provider with consent-based OAuth/Link and server-held tokens; never store bank credentials or provider access tokens in the APK.
- Use provider webhooks to ingest changes server-side; send generic push notifications without merchant or amount on lock screens. Do not assume any bank supports a specific provider without verification.
- Use encrypted server storage, least-privilege roles, MFA, revocation, export/deletion controls, backups, and audit logging.
- Decide whether either spouse can delete transactions or only archive/correct them; prefer reversible edits.
- Resolve offline edits with version numbers, conflicts, and an explicit sync status.
- Never treat savings transfers as income or classify merchant spend as recurring by default.

## Security limitations
The current version uses JavaScript localStorage and is NOT an appropriate long-term store for sensitive household financial records. Move data to a secured database and implement authentication before live bank integration. No INTERNET permission is included in this local-only version.

## Build an APK on GitHub (no Android Studio required)
1. Create a **private** GitHub repository called `HomeLedger-Android` (do not initialize with README).
2. Unzip `HomeLedger_GitHub_APK_Build.zip` and upload **the contents** of the folder, including `.github/workflows/build-apk.yml`, to the repository root. On GitHub, **Add file > Upload files** supports dragging the extracted files/folders from a computer. If uploading on a phone, the GitHub web interface may not preserve hidden `.github` folders; use a computer browser or Git client.
3. Commit the files to the `main` branch.
4. Open **Actions > Build HomeLedger APK > Run workflow** (or let the push-triggered run finish).
5. Open the successful run, download **HomeLedger-debug-APK** from Artifacts, extract its ZIP, then install `app-debug.apk` on Android.
6. Android may ask you to allow installation from the app used to open the APK. Enable this only for the APK you built yourself.

This is a debug-signed, local-only prototype APK. Do not publish or store sensitive bank information in it. It has not been compiled or tested here; inspect the workflow logs if the first build fails. GitHub's free Actions quota and artifact retention depend on account and repository settings. Do not upload personal transaction exports to the repository.

# Pocket Lawyer — Android Studio Project (v1.2, recovered)

Kotlin + Jetpack Compose. Package: `com.pocketlawyer.in` · minSdk 26 · targetSdk 36.

## How to open and run (do these in order)
1. **Uninstall any old "Pocket Lawyer" from your phone first** (this is the most common reason
   an old/unfinished version keeps appearing: Android won't replace an app signed with a
   different key).
2. Unzip. In Android Studio choose **File → Open** and select the `PocketLawyer` folder that
   contains `settings.gradle` (do NOT use "New Project").
3. Let Gradle sync finish. Android Studio adds the `sdk.dir` line to `local.properties` itself.
4. **Build → Clean Project**, then **Build → Rebuild Project**.
5. Plug in the phone (USB debugging on) and press **Run**.
6. To make the submission file: **Build → Generate Signed Bundle / APK** → choose the keystore
   `app/pocketlawyer.jks` (alias `pocketlawyer`, password `pocketlawyer123`).

## Where things are configured
| Thing | Location |
|---|---|
| AI (Gemini) API key | `local.properties` → `AI_API_KEY=...` |
| Gemini model list / retries | `app/src/main/java/.../network/ApiConfig.kt` |
| Chatbot instructions (system prompt) | `.../screens/ChatViewModel.kt` → `buildSystemPrompt()` |
| Google Sign-In Web Client ID | `app/build.gradle` → `GOOGLE_WEB_CLIENT_ID` |
| English / Tamil / Hindi text | `app/src/main/res/values`, `values-ta`, `values-hi` |

## Google Sign-In
Sign-in only works when the app's signing SHA-1 is registered as an **Android OAuth client**
(package `com.pocketlawyer.in`) in the same Google Cloud project as the Web Client ID above.
Both Debug (Run button) and Release builds are signed with `app/pocketlawyer.jks`, so one
fingerprint covers both:

    SHA-1: A0:25:C7:AB:C7:DC:36:A1:0D:9D:B8:A3:57:E5:8E:FB:B8:65:58:E0

If Google login shows an error, check that this SHA-1 is listed in Google Cloud Console →
APIs & Services → Credentials. "Continue as Guest" always works without it.

## What was fixed/added in v1.2 (on top of the recovered project)
- **SOS now sends the SMS automatically** (SmsManager) with a fresh GPS location link; if SMS
  permission is refused it opens the SMS app with the message pre-filled instead.
- **AI chat**: follows the language selected in the app (English/Tamil/Hindi), simpler
  step-by-step answers, fixed a bug that sent the user's question twice, automatic fallback
  to other Gemini models, safer response parsing, localized error messages.
- **Complaint PDF**: footer disclaimer no longer runs off the page.
- **Language** now persists after restarting the app on Android 12 and below; Android 13+
  shows the app in the system per-app language settings.
- **Guest** greeting/profile are translated ("Hello, Guest" / Tamil / Hindi).
- **Profile photo** is copied into app storage so it still shows after restart.
- Debug builds use the release keystore (see Google Sign-In above).

## Notes
- The API key lives in `local.properties` (not committed to git). Anything inside an APK can
  be extracted by determined users, so for a public release route AI calls through your own
  server.
- Google Play restricts the `SEND_SMS` permission to specific app types; for a Play Store
  release you would need to declare/justify it (or switch back to the SMS-composer fallback).
- Test SOS on a real phone with a SIM; emulators can't send real SMS.

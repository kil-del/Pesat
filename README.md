# PesaTrack: Phase 1 (project setup + phone auth)

## Setup
1. Open in Android Studio (latest stable). Let Gradle sync; bump versions in `gradle/libs.versions.toml` if prompted.
2. Firebase console: create a project, add Android app `com.pesatrack`, put `google-services.json` in `app/`.
3. Authentication > Sign-in method > enable **Phone**. Add test phone numbers for development.
4. Add SHA-1 and SHA-256 of debug and release keys (`./gradlew signingReport`). Phone Auth fails without them.
5. Enable the **Play Integrity API** for the project (reCAPTCHA is the fallback).
6. Deploy rules: `firebase deploy --only firestore:rules`.
7. Run: `./gradlew installDebug`. Unit tests: `./gradlew testDebugUnitTest`.

## Assumptions
- Phase 1 uses a state-driven auth gate; Phase 2 adds the full Navigation Compose graph and bottom nav.
- OTP auto-read uses the **SMS User Consent API** (no READ_SMS permission). The hash-based SMS Retriever needs a custom SMS format, which Firebase does not allow.
- PIN = salted PBKDF2 hash in EncryptedSharedPreferences; every 5 wrong tries triggers an escalating lockout.
- Biometric prompt, auto-lock timer, logout and delete-account UI are scaffolded in the ViewModel/repository and finished in Phase 2 with Settings.
- Not yet included: launcher icon, Hilt-injected locale switching, SQLCipher/Room (Phase 2), Firestore sync (Phase 5), Baseline Profile (Phase 5).

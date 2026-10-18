# Readstride — Android engineering notes

Readstride is a reading-practice app built with React Native and TypeScript under BP96 STUDIO. The source repository is private; this page describes the implementation without distributing source code or signing credentials.

**Status, October 2026:** version 1.1.0 is available to invited testers through Google Play internal testing. This is not a claim of a public production release. Advertising approval, regional consent flows, and real purchase/restore behavior still require release validation.

## Product scope

- Seven interface languages: English, Turkish, Spanish, Brazilian Portuguese, Indonesian, German, and French; English is the default for new installations.
- Reading exercises and a 65-text catalog. Reading texts currently cover English and Turkish, rather than all seven interface languages.
- Classic reading selections with source and rights documentation.
- Local reading progress and settings, with existing user data retained across upgrades.
- Interstitial and optional rewarded-ad integrations, and a one-time Premium purchase integration.

## Engineering decisions

### Preserve state across launches and upgrades

The storage layer uses asynchronous file operations with temporary writes and a backup. Release testing exposed a problem in optimized Android builds involving Kotlin record conversion and reflection. The fix preserves the required reflection metadata and classes while retaining general R8 optimization, and uses the compatible asynchronous filesystem API.

Validation covered first launch, saving, force-stop/relaunch, and retaining an existing language preference. This is one reason to test the signed release build as well as the development build.

### Treat localization and reading content separately

The interface supports seven languages, while the reading catalog explicitly identifies the language of each text. New installations start in English; a saved user language choice takes precedence during updates.

### Gate monetization on user state

Advertising depends on age and consent state. Interstitials are limited to natural exercise boundaries, with initial-use grace and frequency caps. Rewarded access requires an explicit user choice and a completed reward event. Premium is designed to remove ads and unlock additional content.

These describe implemented integration rules, not a guarantee of live ad delivery or a completed payment verification.

### Keep release identity stable

The Android package identity and signing continuity remain stable while version codes increase. The workflow is to upload a signed bundle to internal testing, check it on a physical phone, and promote the approved artifact to production.

## Validation boundaries

The build has passed TypeScript checks, targeted linting, 15 automated tests, and exercise-generation checks covering 33,600 rounds. Native release checks also covered cold-start persistence and 16 KB native-library alignment. These checks do not replace physical-device testing, regional consent validation, or Google Play Billing tests.

[App information, support, and privacy policy](https://yasinakgulbp.github.io/)

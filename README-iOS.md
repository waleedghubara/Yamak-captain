# Yamak Kaptan (يمك كابتن) — iOS build & App Store upload

This package builds **only the driver app**:

| App | Display name | Bundle ID | EAS build profile |
|---|---|---|---|
| Driver (captain) | يمك كابتن | `com.waleedghubara.yamak.captain` | `production` |

- No `APP_VARIANT` needed: this package is fixed to the captain app (`app.config.ts`).
- Version: `1.3.0`. Build numbers are managed by EAS (`"appVersionSource": "remote"`).
- Backend: `https://api.yamaktaxi.com` (in `yamak.config.json`; do not change).
- `ios/` and `android/` are generated (Continuous Native Generation). Do not edit them by hand.
- iOS bundle identifier can be overridden with `IOS_BUNDLE_IDENTIFIER`; the default is
  `com.waleedghubara.yamak.captain`.
- The `captain-ios` Codemagic workflow uploads builds to TestFlight and uses Codemagic's
  `BUILD_NUMBER` for the iOS build number. App Store review submission remains disabled.
- The Android application ID remains `iq.yamak.captain`.
- The customer app (يمك تكسي, `iq.yamak.app`) is a separate package.

## What's new in 1.3.0
- Fixed captains being logged out after swiping the app away while online (background location).
- Admin announcements pop up inside the app + a Notifications screen (bell icon on the home screen).
- Balance shown as green "current balance" (turns red as it runs out) or red "due to company".
- Rotating ad banner.
- New dependency: `expo-live-activity@0.4.2` (used by the customer app; nothing to set up for the captain app). Run `npm ci` before building.

## Requirements
- Node.js 20 or newer, npm
- An Apple Developer account with an App Store Connect app record for `com.waleedghubara.yamak.captain`
- An Expo (EAS) account for the cloud build, **or** a Mac with Xcode for a local build

## Option A — EAS cloud build (recommended, no Mac needed)
```bash
npm ci
npx eas-cli@latest login

# Using your own Expo account? Clear "easProjectIdCaptain" in yamak.config.json first,
# then link the project and paste the printed ID back into that field:
npx eas-cli@latest init

npx eas-cli@latest build -p ios --profile production
npx eas-cli@latest submit -p ios --profile production
```
(or `npm run build:ios` and `npm run submit:ios`)

## Option B — Local Xcode build (Mac)
```bash
npm ci
npx expo prebuild -p ios --clean
cd ios && pod install && open *.xcworkspace
# Xcode: select your Team -> set Build number -> Product > Archive -> Distribute App -> App Store Connect
```

## Notes for App Review
- **Login:** phone number + 6-digit code (normally sent on WhatsApp). Provide any
  App Review test-account details to Apple through the appropriate private channel.
- **Background location:** used only while the driver is Online (receiving requests) or on an active trip,
  so nearby requests reach the driver and the customer can follow the car. It stops when the driver goes
  Offline. iOS shows the location indicator while it is active.
- **Account deletion:** Account tab > Delete account.
- **Encryption:** `ITSAppUsesNonExemptEncryption = false` is already set.
- **Payments:** customers pay the fare in cash; the driver pays Yamak a fixed 250 IQD fee per trip outside
  the app. No in-app purchases.

---

## بالعربي (مختصر)
- هذا الملف لتطبيق **يمك كابتن** بس (`com.waleedghubara.yamak.captain` على iOS، و`iq.yamak.captain` على Android).
- البناء الأسهل بـ EAS: `eas build -p ios --profile production`، أو بـ Xcode على ماك.
- تُرسل بيانات حساب المراجعة إلى Apple عبر القنوات الخاصة المناسبة، ولا تُحفظ في المستودع.

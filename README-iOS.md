# Yamak Kaptan (يمك كابتن) — iOS build & App Store upload

This package builds **only the driver app**:

| App | Display name | Bundle ID | EAS build profile |
|---|---|---|---|
| Driver (captain) | يمك كابتن | `com.waleedghubara.yamak.captain` | `production` |

- No `APP_VARIANT` needed: this package is fixed to the captain app (`app.config.ts`).
- Version: `1.4.0`. Build numbers are managed by EAS (`"appVersionSource": "remote"`).
- Backend: `https://api.yamaktaxi.com` (in `yamak.config.json`; do not change).
- `ios/` and `android/` are generated (Continuous Native Generation). Do not edit them by hand.
- iOS bundle identifier can be overridden with `IOS_BUNDLE_IDENTIFIER`; the default is
  `com.waleedghubara.yamak.captain`.
- The `captain-ios` Codemagic workflow uploads builds to TestFlight and uses Codemagic's
  `BUILD_NUMBER` for the iOS build number. App Store review submission remains disabled.
- The Android application ID remains `iq.yamak.captain`.
- The customer app (يمك تكسي, `iq.yamak.app`) is a separate package.

## What's new in 1.4.0 (read before building)
Full technical list: **`CHANGES-1.4.0.md`** in this package.

- **New dependencies — run `npm ci` again:** `expo-sharing@~57.0.26`, `expo-audio@~57.0.5`, `expo-asset@~57.0.18`.
  `expo-asset` is pinned deliberately; without the pin npm hoists a newer copy and the JS bundle fails to build.
- **New iOS extension — customer app only.** `expo-sharing` adds a Share Extension so a customer can share a WhatsApp location straight into the app.
  Target `expo-sharing-extension`, bundle ID **`com.waleedghubara.yamak.customer.expo-sharing-extension`**, App Group **`group.com.waleedghubara.yamak.customer`**.
  Run the **first** customer build interactively (no `--non-interactive`) and sign in to Apple when asked, so EAS registers the App ID and the App Group. The customer app now has **two** extensions (this one and Live Activity).
- **Custom notification sound — captain app only:** `assets/yamak_ring.wav` registered through `expo-notifications`. It is the ride-offer ring (three rings, then it stops).
- **New Info.plist key:** `LSApplicationQueriesSchemes = ["maps", "comgooglemaps", "waze"]`, so the captain app can hand the route to Apple Maps / Google Maps / Waze.
- **Dark mode:** `userInterfaceStyle` is now `automatic` (was `light`). Both apps follow the iPhone appearance setting; every screen was restyled for it.
- **Khatti (shared lines)** in both apps, **one ride category** instead of taxi/private, rebuilt captain home screen, promo codes applied with one tap, captain rewards, offline caching and an update gate.
- Unchanged: bundle identifiers, the Live Activity extension, EAS profiles, and the backend URL.

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

## Store screenshots

`screenshots/` holds the 1.4.0 store images for both variants (6 customer + 6 captain)
plus each variant's 512x512 icon. See `screenshots/README.md` for the screen-by-screen list.
All personal data inside the images is demo data.

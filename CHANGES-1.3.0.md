# Yamak Captain 1.3.0 — iOS update

This is your repository (waleedghubara/Yamak-captain) with the 1.3.0 changes applied on top.
Your settings are kept: bundle ID `com.waleedghubara.yamak.captain`, Codemagic workflow
`captain-ios` (workspace/scheme `ymkkabtn`), build number from `BUILD_NUMBER`, EAS profiles.

## Build
Same as before (Codemagic `captain-ios`). Version is now **1.3.0** (app.config.ts).
`npm ci` installs one new package, `expo-live-activity@0.4.2`: it is used by the customer app;
the captain app needs no extra target, capability or signing step.

## What changed
- Fixed captains being logged out after swiping the app away while online
  (the background location update was sent before the saved login was read, and a 401 wiped the session).
- Admin announcements pop up inside the app + Notifications screen (bell icon on the home screen).
- Balance shown as green "current balance" (turns red as it runs out) or red "due to company".
- Rotating ad banner.

## Checked before sending
`npm ci`, `tsc --noEmit` and `expo prebuild -p ios` succeed; bundle ID and project name unchanged.

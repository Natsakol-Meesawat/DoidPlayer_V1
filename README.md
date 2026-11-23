# DoidPlayer (Android TV Box)

Kotlin + AndroidX Media3 ExoPlayer. Minimum Android 10 (API 29), target API 34.

## Media files
On first launch, Android system file picker asks you to select video 1 and then video 2 (MP4 recommended). The app stores persisted read access to both files. Keep the selected files in place.

The app starts `video1.mp4` in an infinite loop. Touch/click the screen or send `PLAY2` to play `video2.mp4` once, then return to looping video 1.

## UDP control
The app listens on UDP port **5000**. Send UTF-8 command text:
- `PLAY1` — loop video 1
- `PLAY2` — play video 2 once, then loop video 1
- `STOP` — stop playback
- `PAUSE` — pause
- `RESUME` — resume

Example (Linux/macOS): `echo -n PLAY2 | nc -u -w1 BOX_IP 5000`

## Build
Requires JDK 17 and Android SDK platform 35 / build-tools. From project root:
`gradle assembleDebug`

APK: `app/build/outputs/apk/debug/app-debug.apk`

GitHub Actions builds a debug APK on push/PR or manual workflow dispatch; download it from the workflow run's Artifacts.

## Boot launch note
The app declares a BOOT_COMPLETED receiver. Android TV vendors may restrict background activity launches; if the box blocks it, enable DoidPlayer under the device's Auto Start / Startup apps setting. First install and launch the app once.

## Notes
- UDP server runs while the activity is alive; Android may terminate the process under memory pressure.
- For reliable unattended deployment, set the box to keep DoidPlayer as the launcher/kiosk app if supported.
- Media selection uses Android Storage Access Framework with persisted URI permissions, compatible with scoped storage on Android 10 to 14.

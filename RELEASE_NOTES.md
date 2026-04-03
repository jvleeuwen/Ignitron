# Release Notes

## 1.9.1.7 - 2026-04-03
- Fixed iOS Spark app disconnect loop where app subscribed and then disconnected if amp was not yet connected.
- Updated BLE server connect flow to keep/start amp scan while app is connected but amp is still offline.
- Stop scan only when both app and amp links are active to reduce radio contention safely.

## 1.9.1.6 - 2026-04-03
- Improved iOS Spark app connection stability by stopping active BLE scan when app connects to Ignitron server.
- Removed immediate re-advertise-on-connect behavior that could destabilize app connections.
- On app disconnect, advertising restarts and amp scan resumes only when needed.

## 1.9.1.5 - 2026-04-03
- Added and initialized `RELEASE_NOTES.md`.
- Established per-change release note tracking for each subversion update.

## 1.9.1.4 - 2026-04-03
- Reduced TFT blinking during preset retrieval by marking the display as dirty only when visible state changes.
- Removed BLE keyboard initialization from APP mode to avoid BLE stack conflicts with iPhone Spark app connections.

## 1.9.1.3 - 2026-04-03
- Improved Spark app connection compatibility by relaxing strict BLE Secure Connections requirement in server startup.
- Added BLE advertising preferred connection parameters for better mobile compatibility.

## 1.9.1.2 - 2026-04-03
- Implemented event-driven TFT refresh path with mode/submode tracking.
- Added one-time full clear after splash/mode transitions to remove stale logo pixels.
- Reduced unnecessary full-area redraws to lower visible flicker.

## 1.9.1.1 - 2026-04-03
- Switched to 4-part versioning format (`major.minor.patch.subversion`) to support per-change subversion bumps.

# Release Notes

## 1.9.1.36 - 2026-04-03
- Improved immediate profile visibility after connect/preset changes by pausing background missing-HW-preset sync briefly after a preset switch event.
- Reduced background HW cache synchronization burst load by requesting only one missing HW preset per sync cycle instead of iterating all missing presets in one pass.

## 1.9.1.35 - 2026-04-03
- Fixed APP-mode startup crash (`std::vector::at` out-of-range) caused by invalid preset index `0` reaching hardware-preset cache lookup during preset-number update handling.
- Added defensive preset-index validation in preset builder and ignored invalid Spark preset-number updates (< 1) before triggering hardware-preset activation logic.

## 1.9.1.34 - 2026-04-03
- Fixed APP-mode first-connection startup race that could trigger profile/preset requests before Spark notification subscription was active, causing an initial subscribe failure/reconnect cycle.
- Added explicit amp-notification-subscription tracking in BLE client state and gated boot-time response-chain requests (`amp name`/`checksums`/`current preset`) to run only when notifications are confirmed active.

## 1.9.1.33 - 2026-04-03
- Fixed APP-mode startup profile sync race where Ignitron could miss the amp's initial profile/preset responses because the first request was sent before notification subscription completed.
- Moved initial state bootstrap to post-subscribe and now explicitly requests amp name, current preset number, and current preset right after BLE connection is established, so the active profile is shown reliably.

## 1.9.1.32 - 2026-04-03
- Hardened APP-mode BLE amp connect flow by always using a fresh client for each selected Spark candidate and rejecting mismatched peers without disconnect/re-scan churn, reducing startup crash loops when amp is already on.
- Improved tuner exit stability by adding a short post-off block before raw 25-byte tuner-control frames can re-arm Ignitron auto-enter, preventing immediate tuner re-entry after leaving tuner in Spark app.
- Fixed large-screen tuner center-note rendering by removing the blank special-case mapping for note value `0x0e` so note letters are shown consistently.

## 1.9.1.31 - 2026-04-03
- Fixed APP-mode startup preset sync so Ignitron no longer restores and pushes the last cached preset to the amp on boot; it now reads the amp's current preset instead, preventing unintended CH2 -> CH1 changes.
- Hardened BLE client connect/subscribe flow to reject mismatched peer addresses before notification subscription, avoiding crashes when a bogus scanned peer is returned after a failed connection attempt.

## 1.9.1.30 - 2026-04-03
- Fixed APP-mode Ignitron tuner entry by arming tuner auto-enter from the raw 25-byte app control frames that precede tuner output, instead of relying only on parsed request classification.
- Matches the actual Spark app tuner-start traffic pattern seen in logs where tuner output was forwarded correctly but Ignitron never switched into tuner mode.

## 1.9.1.29 - 2026-04-03
- Replaced the APP-mode tuner auto-entry timing heuristic with an explicit arm/disarm state: tuner output can open Ignitron tuner only after a fresh app tuner request, and explicit tuner-off now disarms further re-entry until the next tuner request.
- Removed the incorrect preset-response-based tuner exit fallback.

## 1.9.1.28 - 2026-04-03
- Fixed APP-mode tuner UI getting stuck after leaving tuner in Spark app by exiting tuner mode when normal preset/hardware-preset responses resume.
- Restored Ignitron tuner auto-entry on tuner output frames while preserving the short post-exit cooldown that blocks immediate bounce-back from trailing tuner packets.

## 1.9.1.27 - 2026-04-03
- Fixed APP-mode crash under high tuner traffic by adding bounded queue backpressure for BLE notification buffering and dropping oldest queued blocks when overloaded.
- Improved APP-mode responsiveness by processing multiple queued Spark blocks per update cycle instead of only one.
- Reduced bridge log overhead in high-rate paths (especially tuner output) to avoid BLE/CPU contention while Spark app is connected.
- Refined tuner mode transitions: tuner output now auto-enters tuner only in a short window after a recent app tuner request, while explicit tuner-off clears that arm state.

## 1.9.1.26 - 2026-04-03
- Fixed APP-mode tuner entry regression where Ignitron did not enter tuner mode if Spark app/amp only emitted tuner output (`03 64`) without an explicit tuner-on event (`03 65 on`).
- Added a short cooldown after tuner-off so trailing tuner-output packets do not immediately re-open tuner mode.

## 1.9.1.25 - 2026-04-03
- Fixed APP-mode tuner exit behavior where late tuner-output frames could force Ignitron back into tuner submode after tuner was turned off in Spark app.
- Tuner submode is now toggled only by explicit tuner on/off messages.

## 1.9.1.24 - 2026-04-03
- Fixed APP-mode tuner handling to stop sending `switchTuner(false)` when tuner output is received while not in tuner submode.
- Tuner on/off messages from amp now update Ignitron submode/display locally without echoing redundant tuner commands back to amp.

## 1.9.1.23 - 2026-04-03
- Fixed APP-mode disconnect on tuner/submode events by preventing BLE keyboard start/stop calls outside KEYBOARD mode.
- Avoids `Stopping advertising keyboard` side effects during Spark app sessions.

## 1.9.1.22 - 2026-04-03
- In APP mode, completed amp responses are now forwarded to the Spark app while connected even if pending request bookkeeping becomes temporarily out of sync.
- Added pending-request queue reset on Spark app disconnect to prevent stale message IDs from causing ignored responses after reconnect.

## 1.9.1.21 - 2026-04-03
- Hardened BLE scan candidate selection to skip empty-name/non-Spark candidates that falsely advertise the Spark service UUID and can crash notification subscription.
- Disabled local APP-mode auto-sync requests (amp name/checksum/current-preset fetches) while Spark app is connected to reduce bridge contention and app disconnects.

## 1.9.1.20 - 2026-04-03
- Added raw APP-mode passthrough of all app-originated Spark frames to the amp, not only request packets.
- Fixes missing bridge forwarding for control commands (such as tuner and preset/effect changes) that left the app connected but did not update amp state.

## 1.9.1.19 - 2026-04-03
- Fixed APP-mode amp-response forwarding to send original raw Spark BLE frame blocks to the Spark app instead of parsed payload chunks.
- Resolves malformed app notifications (`ign->app` with invalid headers) that caused the app to unsubscribe/disconnect after initial requests.

## 1.9.1.18 - 2026-04-03
- Paused background missing-HW-preset sync while Spark app is connected in APP mode.
- Reduces handshake and request/response contention that can cause the app to unsubscribe/disconnect during startup.

## 1.9.1.17 - 2026-04-03
- Fixed APP-mode request forwarding to rebuild valid amp requests from parsed Spark app request types instead of sending empty payloads.
- Replaced single pending-request tracking with multi-request tracking so back-to-back app startup requests can be matched to amp responses correctly.

## 1.9.1.16 - 2026-04-03
- Fixed the temporary BLE bridge diagnostics to use the actual parser status enum values exposed by `SparkStreamReader`.
- Resolves the build failure introduced by the first debug-logging pass.

## 1.9.1.15 - 2026-04-03
- Added targeted APP-mode BLE bridge diagnostics for app writes, amp writes, amp notifications, parser outcomes, and app notifications.
- New logs make it possible to correlate each bridge step and see whether a response was forwarded, ignored, or never arrived.

## 1.9.1.14 - 2026-04-03
- Restricted APP-mode response forwarding so the Spark app only receives amp responses that match app-originated requests.
- Prevents unsolicited Ignitron background amp traffic from being pushed to the app and destabilizing the BLE session.

## 1.9.1.13 - 2026-04-03
- Forwarded completed amp-originated messages and acknowledgments back to Spark app in APP mode.
- Completes the APP-mode BLE bridge path so Spark app receives responses instead of timing out after subscribe.

## 1.9.1.12 - 2026-04-03
- Added APP-mode request forwarding from Spark app BLE server path to Spark amp BLE client path.
- Prevents Spark app timeout/disconnect after subscribe caused by missing request bridge handling.

## 1.9.1.11 - 2026-04-03
- Fixed advertised-device lifetime handling by copying scan results into owned storage before connect.
- Added connected-client guard before notification subscription service lookup to prevent crash in `getService`.
- Finalized APP-mode amp connection stability around scan/connect/subscribe sequencing.

## 1.9.1.10 - 2026-04-03
- Fixed amp connection crash during notification subscription (`NimBLEClient::getService`) by hardening client/descriptor checks.
- Added self-advertisement filtering during BLE scan to avoid self-connect loops.
- Fixed APP-mode BLE connection success flow to only mark connected after notification subscription succeeds.
- Repaired malformed BLE callback block and restored correct `startServer()` implementation.

## 1.9.1.8 - 2026-04-03
- Adjusted BLE server app-connect callback to avoid scan state changes during iOS GATT setup.
- Added unsubscribe diagnostic logging with amp-connection status to help trace Spark app disconnect cause.

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

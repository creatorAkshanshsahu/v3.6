# AK TV v3.5 — Shaka playback experiment

## What changed
- Adds a Shaka Player playback page bundled in `app/src/main/assets/shaka_player.html` and loaded through GeckoView’s `resource://android/assets/` scheme.
- Runs Shaka inside the app's existing bundled GeckoView engine. This does **not** use Android System WebView and does not load the TG TV embed page/iframe.
- Routes channel playback through Shaka while retaining the existing native AKTV home UI, channel guide, programme details, and remote-control channel navigation.
- Fetches `GET https://livetgtv.lovable.app/api/public/channels/{id}` when a channel starts, preserves each signed source URL exactly as returned, tries every source in the response, and applies ClearKey values from the current response.
- Adds retry parameters and a visible failure message instead of leaving the playback surface silently black.

## Important implementation notes
- Shaka Player JavaScript is currently loaded from the jsDelivr CDN (`shaka-player@4.15.18`); the HTML page is local, but the JS library still requires internet access. Bundle the compiled Shaka JS into assets for a fully offline/self-contained player build.
- The API documentation says sources can be signed and short-lived. The page re-fetches the API before playback and once more after all initial sources fail.
- This is an experimental playback-engine change, not a confirmed fix for Sony Yay Hindi or every TV. A 403 from a media segment may still indicate provider authorization/header/cookie requirements. Capture GeckoView/Shaka network errors and compare with the working website request before adding headers; do not blindly modify signed URLs.
- Programme guide and existing UI remain in the native app.

## Validation status
- Source/package edits only. No Gradle executable or Gradle wrapper JAR is available in this environment, so this project could not be compiled or tested on a physical Android TV here.
- Test first with channel ID `872` (Sony Yay Hindi), then compare at least one known-working channel. Confirm GeckoView supports MSE/EME/ClearKey on the target TV and that the Shaka CDN is reachable.

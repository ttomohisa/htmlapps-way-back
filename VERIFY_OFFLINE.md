# Offline / standalone verification

Way Back is distributed as self-contained HTML, but its core geolocation behavior depends on browser security policy.

## What can be verified offline

- `dist/index.html` opens without external scripts, styles, images, fonts, or frames.
- The UI, saved-place data model, dialogs, language switching, and LocalStorage code are contained in one file.
- The generated CSP includes `connect-src 'none'`.
- `dist/index.self-extract.html` restores the readable HTML in browsers supporting `DecompressionStream`.

## Geolocation limitation

Geolocation and device orientation are commonly restricted to secure contexts. A `file://` copy may render correctly while location/compass APIs remain unavailable. Test actual navigation from HTTPS (for example GitHub Pages or Browser Kitty) on the target smartphone.

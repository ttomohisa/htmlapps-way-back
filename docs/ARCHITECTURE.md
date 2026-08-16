# Architecture

Way Back follows the `htmlapps-template` repository model.

```text
app.config.json
APP_SPEC.md
dependencies.json
src/index.template.html
build-standalone.ps1
scripts/build-self-extract.ps1
scripts/verify-standalone.ps1
scripts/check-repository.ps1
dist/index.html
dist/index.self-extract.html
```

## Runtime

The release is one HTML document. Geolocation tracking, bearing/distance math, compass handling, translations, persistence, dialogs, sharing, and Wake Lock integration are inline. No third-party package is currently embedded.

The CSP blocks application runtime network connections with `connect-src 'none'`.

## Location pipeline

1. The user taps **Start location**.
2. The app starts `navigator.geolocation.watchPosition()` with high-accuracy mode enabled.
3. Current position samples remain in memory and populate a short recent-sample window.
4. When the user saves a place, the app selects the recent sample with the smallest reported `coords.accuracy` value.
5. Saved coordinates, accuracy, timestamp, and user-provided name are written to LocalStorage.
6. Distance is calculated with the haversine formula and bearing with spherical forward-bearing math.
7. Current and saved-position uncertainty are combined into a practical arrival radius, clamped to 12–80 m.

## Compass pipeline

- On platforms exposing `DeviceOrientationEvent.requestPermission()`, permission is requested from the same transient user gesture that starts location.
- `webkitCompassHeading` is preferred when supplied by the browser.
- Absolute device-orientation alpha is used as a fallback where available.
- Heading is low-pass smoothed using shortest-angle interpolation to reduce jitter around 0°/360°.
- If heading is unavailable or permission is denied, the app remains functional and renders the target bearing relative to north.

## Privacy boundary

Current location is not persisted by default. Only explicit saved-place actions write coordinates to LocalStorage. The page performs no application-level fetch/XHR/API upload. Platform location services may independently use network-assisted positioning according to OS/browser settings.

## Build placeholders

`src/index.template.html` contains exactly one of each:

- `__APP_CONFIG_JSON__`
- `__BUILD_MANIFEST_JSON__`
- `__EMBEDDED_ASSET_BUNDLE_BASE64__`

# APP_SPEC.md

## 1. Product identity

- **Name:** Way Back
- **Purpose:** Save a place on a smartphone and guide the user back with distance and direction, without requiring a map or native app installation.
- **Primary users:** Smartphone users who want to remember a parking spot, tent, meeting point, entrance, or temporary location.
- **Release artifacts:** `dist/index.html` and `dist/index.self-extract.html`.

## 2. Core outcome

1. Open the page over HTTPS on a smartphone.
2. Start location access and grant geolocation permission.
3. Save the current place with a short name.
4. Later select that place and see distance plus a large directional arrow.
5. If device heading is unavailable, continue with north-referenced bearing and distance.

## 3. Functional requirements

- Continuous current-location tracking with `navigator.geolocation.watchPosition()`.
- Save multiple named places in LocalStorage.
- Automatically choose the most accurate location fix from the recent 15-second sample window when saving.
- Show straight-line distance and bearing from the current position to the active saved place.
- Use device heading when available; gracefully fall back to north-referenced direction when unavailable or denied.
- Smooth heading changes to reduce compass jitter.
- Show current GPS accuracy, saved-point accuracy, and an accuracy-aware arrival zone.
- Arrival radius is based on combined current/saved uncertainty, clamped to a practical 12–80 m range.
- Share a saved point through Web Share when available, with clipboard fallback.
- Keep screen awake during active guidance when Wake Lock is available and enabled.
- Japanese and English in the same HTML.
- Light-only, touch-first smartphone UI.
- No third-party runtime dependency.

## 4. Data and privacy

- Current location is processed in memory and is not persisted unless the user explicitly saves a place.
- Saved place name, coordinates, accuracy, timestamp, active selection, language, and Wake Lock preference are stored in LocalStorage when available.
- The app does not send coordinates to a server and performs no application-level runtime network request.
- Browser/OS positioning services may use device network services according to platform settings; the app does not control that behavior.
- CSP retains `connect-src 'none'`.

## 5. UX decisions

- No embedded map. The product is intentionally a lightweight “return to this point” tool rather than a navigation-map replacement.
- The main smartphone screen prioritizes a large arrow, distance, and arrival status.
- Saving is one dialog with a name and quick-name presets.
- Multiple points are available through a bottom-sheet-style saved-place list on smartphones.
- GPS uncertainty is visible instead of presenting false precision.
- Location failures must be explicit. The app must not look active while no real geolocation sample has been received.

## 6. Browser / platform behavior

- Target current stable Chromium, Safari, and Firefox where geolocation is available.
- Geolocation normally requires a secure context (HTTPS) and explicit user permission.
- The HTML may open through `file://`, but geolocation is not guaranteed there; HTTPS is the supported runtime environment.
- Device orientation/compass support varies. Way Back must remain useful without it.
- DeviceOrientation permission, when required, must be requested directly from the same user gesture that starts the app.
- Wake Lock is optional and must fail gracefully.

## 7. Limitations

- GPS accuracy can degrade indoors, underground, or around tall buildings.
- Device heading can be affected by magnetic interference and browser implementation differences.
- The app is not for emergency, marine, aviation, mountain safety, or other safety-critical navigation.
- Distance is straight-line distance, not walking/driving route distance.

## 8. Acceptance criteria

- Both generated release variants are self-contained.
- No external runtime scripts/styles/fonts/images/frames.
- CSP contains `connect-src 'none'`.
- UI works from 320 px smartphone width through desktop.
- Japanese/English switching does not reload.
- Start is disabled with a clear reason when geolocation is unavailable or permission is known to be denied.
- A real location sample is required before save controls become active.
- Saving chooses the best recent accuracy fix and persists the point locally.
- Distance/bearing update as current location changes.
- Heading permission denial does not prevent distance and north-referenced guidance.
- Delete-all and individual delete use in-app confirmation UI.

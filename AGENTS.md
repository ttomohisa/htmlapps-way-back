# AGENTS.md — Way Back / Single HTML App Contract

Read `APP_SPEC.md`, `docs/ARCHITECTURE.md`, and the current `src/index.template.html` before editing.

## Non-negotiable constraints

- Produce `dist/index.html` and `dist/index.self-extract.html` as one-file release artifacts.
- Do not add runtime CDN, remote font, analytics, telemetry, remote API calls, or hidden application network dependencies.
- Keep `connect-src 'none'` in the release CSP.
- Use browser-native APIs where practical. Third-party dependencies must be pinned and declared in `dependencies.json`.
- Smartphone UX is the priority; desktop must remain usable.
- Keep Japanese and English in the same HTML.
- Keep visible focus, labels / accessible names, sufficient contrast, and reduced-motion handling.
- Do not use generic emoji as primary UI icons; use inline SVG.
- Do not hand-edit generated `dist/` files. Edit source/config/build scripts and rebuild.
- Geolocation and device orientation may require HTTPS. `file://` must still render and clearly explain when those APIs are unavailable.
- Current location must not be persisted unless the user explicitly saves a place.

## Source organization

- Main editable source: `src/index.template.html`.
- Keep the three build placeholders exactly once: `__APP_CONFIG_JSON__`, `__BUILD_MANIFEST_JSON__`, `__EMBEDDED_ASSET_BUNDLE_BASE64__`.
- Keep `APP:BEGIN` / `APP:END` and `APP:HELP:BEGIN` / `APP:HELP:END` markers.
- Keep geodesic distance, bearing, arrival-radius, and compass fallback logic readable enough to audit.

## Required verification

Run on Windows:

```powershell
powershell.exe -NoLogo -NoProfile -ExecutionPolicy Bypass -File .\scripts\check-repository.ps1
```

Also verify location permission, compass behavior, and GPS accuracy on real smartphones over HTTPS. Desktop/headless simulation can validate UI and math but cannot validate physical GPS or magnetometer quality.

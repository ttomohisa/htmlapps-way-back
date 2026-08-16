# Security and privacy

Way Back is designed as a local-first browser utility.

## Location data

- Current geolocation samples are kept in memory.
- Coordinates are persisted only when the user explicitly saves a place.
- Saved places are stored in browser LocalStorage with their name, coordinates, reported accuracy, and timestamp.
- The application code does not upload coordinates, analytics, telemetry, or usage data to a remote service.

The browser or operating system may use network-assisted positioning services according to platform settings. That positioning behavior is outside Way Back and is not an application-level API request made by this page.

## Runtime network boundary

The generated HTML keeps a Content Security Policy containing `connect-src 'none'` and contains no external runtime script, stylesheet, font, image, iframe, analytics, or tracking dependency.

Explicit user-initiated Web Share operations are handled by the browser/operating system and are outside the app's local storage boundary.

## Reporting issues

Please report security or privacy issues through the repository's GitHub security/reporting channel rather than including sensitive coordinates in a public issue.

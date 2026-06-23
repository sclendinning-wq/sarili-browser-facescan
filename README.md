# sarili-browser-facescan

Browser-based face scanning prototype for Sarili custom eyewear.

A single self-contained `index.html` that uses MediaPipe FaceMesh to scan a face
via the phone's front camera and output four measurements in millimetres plus a
frame-size recommendation. No server, no build step, no dependencies to install.
This is a proof-of-concept to validate the measurement algorithm before the iOS
ARKit build.

## Run it

The camera requires HTTPS (Safari blocks `getUserMedia` on `file://`).

- **GitHub Pages:** enable Pages on the `main` branch, then open the published
  URL in Safari on iPhone.
- **Local:** serve over HTTPS, e.g. `npx serve` behind a tunnel, or any static
  HTTPS host. Opening the file directly will load the UI but the camera won't start.

Add `?debug=true` to the URL to show raw landmark coordinates, per-frame values,
frame/discard counts, and the scale factor.

## What it measures

| Measurement   | Landmarks                          |
|---------------|------------------------------------|
| Face width    | temples 234 ↔ 454                  |
| PD            | iris centres 468 ↔ 473 (`refineLandmarks`) |
| Bridge width  | inner eye corners 133 ↔ 362        |
| Bridge height | nose tip 4 → midpoint of 133/362 (Y only, +3mm offset) |

Scale is derived from an assumed 140mm average face width (no physical
reference object). See the technical brief for the full algorithm and the known
limitations of the focal-length calibration method.

## Scope

Camera on → face detected → four measurements out → frame size recommended.
No accounts, no storage, nothing sent anywhere — all processing is client-side.
Temple length and pantoscopic tilt are deferred to the iOS ARKit build.

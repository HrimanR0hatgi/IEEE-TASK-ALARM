# Rouse — Challenge Alarm

A responsive, installable-style web app for alarms that require a small challenge before they can be dismissed. Built with HTML, CSS, and vanilla JavaScript; no framework or backend is required.

## Features
- Add, edit, delete, enable, and disable alarms.
- Persist alarm settings in browser `localStorage`.
- Choose one of three challenges per alarm: mental maths, code recall, or tile sequence.
- Select easy, medium, or hard difficulty.
- Full-screen ringing view, locally generated repeating tone using Web Audio, and vibration where supported.
- Back-button handling while ringing, validation, storage-error handling, and responsive layout.
- Service-worker shell caching for use after the first visit on a supported HTTPS/static host.

## Run locally

**Recommended:** Python 3 is installed on most development machines.

1. Download/clone this repository.
2. Open a terminal in the project directory.
3. Run:

   ```bash
   python -m http.server 8000
   ```

4. Open `http://localhost:8000` in a modern browser.
5. Add an alarm for one or two minutes ahead, keep the tab open, and allow audio. On a compatible phone, vibration works only if the browser/device permits it.

Opening `index.html` directly (`file://`) may work for the core UI, but service workers require localhost or HTTPS.

## Important browser limitations

This is a browser-based prototype, not a native Android/iOS alarm-clock application. Browsers throttle or suspend background tabs, operating systems can stop browsers, and browser audio/vibration APIs have platform restrictions. Therefore alarms are **not guaranteed to fire if the tab is closed, the browser is suspended, the device is asleep, or the OS kills the browser**. Keep the page active for reliable demonstration. A production alarm app needs native scheduling and alarm/audio permissions (for example, Android exact alarms and a foreground service); a service worker cannot reliably run a continuous alarm at a scheduled time.

The ringing screen intercepts ordinary browser back navigation, but it cannot prevent the user from closing the tab/app or force-stop the browser. Web vibration is supported only on some mobile browsers and generally not on desktop.

## Data and privacy

Alarm configuration is stored only in this browser's `localStorage`. Clearing site data removes it. No account, analytics, API, or server database is used. The app uses Google Fonts when internet access is available; system fallback fonts work offline.

## Quick test checklist

1. Create an alarm with a valid label and time; verify it appears in the list.
2. Reload the page; verify the alarm remains.
3. Edit the label/time/challenge/difficulty; verify changes persist after reload.
4. Toggle an alarm off and on; verify the state is retained.
5. Delete an alarm and confirm the prompt; verify it disappears.
6. Set an enabled alarm one minute ahead, keep the page open, and wait for the full-screen ringing view.
7. Enter a wrong challenge answer; verify sound continues and feedback appears.
8. Solve the challenge; verify the screen closes and sound/vibration stop.
9. Repeat for maths, code recall, and tile sequence, at each difficulty.
10. Try an empty label; verify native required-field validation prevents saving.
11. Test on a mobile browser for vibration support and responsive layout.
12. Test browser back while ringing; verify the ringing screen remains visible.

## Project files

- `index.html` — semantic interface and alarm/challenge screens.
- `styles.css` — responsive visual design.
- `app.js` — persistence, scheduling loop, alarm audio/vibration, challenges, validation, and UI events.
- `manifest.json` — installable-app metadata.
- `sw.js` — static asset caching; it does not schedule alarms in the background.
- `REPORT.md` — implementation explanation and test-record template for the submission document.

# Rouse — IEEE-VIT Task Report

> Replace the screenshot placeholders with screenshots captured from the running application and replace the sample test-record statuses with the actual results observed on your device before submitting. Do not claim a test passed unless you ran it.

## 1. Objective
Build a challenge-based alarm interface that lets users manage alarms locally and requires a selected interactive challenge before stopping the ringing experience.

## 2. Technology stack
- HTML5 for the application structure and forms.
- CSS3 for responsive layout and the full-screen alarm interface.
- Vanilla JavaScript for alarm scheduling, challenge generation, validation, local persistence, and interaction.
- `localStorage` for persistence.
- Web Audio API for generating a continuous tone without downloading an audio file.
- Vibration API for device vibration where supported.
- Service worker for caching the application shell on supported hosts.

## 3. Functional implementation
1. **Alarm CRUD:** Create, view, edit, delete, enable, and disable alarms.
2. **Persistence:** A versioned localStorage key (`rouse.alarms.v1`) stores alarm settings. Invalid stored records are filtered and storage exceptions are caught.
3. **Scheduler:** A foreground JavaScript loop checks the local clock each second. It triggers an enabled alarm when its configured time matches and prevents the same alarm from firing more than once on the same date.
4. **Ringing screen:** A fixed, viewport-filling dialog replaces the normal interface while an alarm is active. A `popstate` handler attempts to keep the screen open when browser Back is pressed.
5. **Audio:** Web Audio generates an alternating square-wave tone. The oscillator and intervals are cleaned up after successful challenge completion.
6. **Vibration:** The Vibration API is called in a repeating pattern where the device/browser supports it. Vibration is stopped when the challenge is completed.
7. **Challenges:**
   - Mental maths: generated arithmetic expression with a numeric answer.
   - Code recall: a random alphanumeric code that must be typed correctly.
   - Tile sequence: tap the highlighted tiles in the generated order.
8. **Difficulty:** Each challenge has three difficulty settings that change arithmetic complexity, code length, or tile-sequence length.
9. **Error handling:** Labels are trimmed and length-limited; time and enum values are validated; localStorage and audio errors are caught; incorrect challenge attempts do not stop the alarm.

## 4. Screenshots to include
- **Figure 1 — Dashboard:** Show the current time and at least two configured alarms.
- **Figure 2 — Alarm configuration:** Show the challenge and difficulty selectors.
- **Figure 3 — Ringing screen:** Show the full-screen alert and one challenge.
- **Figure 4 — Incorrect attempt:** Show error feedback while the alarm remains active.
- **Figure 5 — Successful dismissal:** Show the dashboard/toast after solving the challenge.

## 5. Test record
Record the date, browser/device, actual result, and pass/fail for each test. Suggested table:

| ID | Test procedure | Expected result | Actual result / status |
|---|---|---|---|
| T01 | Create alarm with valid label and time | Alarm appears in dashboard | Fill after execution |
| T02 | Reload page after creating alarm | Alarm persists from localStorage | Fill after execution |
| T03 | Edit alarm fields and save | Updated values are shown and persist | Fill after execution |
| T04 | Disable and re-enable alarm | State updates and persists | Fill after execution |
| T05 | Delete an alarm and confirm | Alarm is removed | Fill after execution |
| T06 | Set alarm one minute ahead with page open | Full-screen ringing view opens at target time | Fill after execution |
| T07 | Submit an incorrect answer | Feedback appears; alarm continues | Fill after execution |
| T08 | Solve challenge correctly | Ringing view closes and tone/vibration stop | Fill after execution |
| T09 | Test all three challenge types | Each challenge can be completed | Fill after execution |
| T10 | Test easy/medium/hard settings | Difficulty changes the task complexity | Fill after execution |
| T11 | Press browser Back while ringing | Ringing view remains visible in supported browser | Fill after execution |
| T12 | Test on mobile browser | Responsive UI; vibration if supported | Fill after execution |
| T13 | Submit blank label | Save is blocked by validation | Fill after execution |

## 6. Known limitations
The current build is a browser prototype. Its scheduler only runs while the page's JavaScript is running; it cannot guarantee alarms when the tab is closed, the browser is suspended, or the device is asleep. Browser Back interception is best-effort and cannot prevent closing the app. Vibration depends on device/browser support and user settings. These constraints require native alarm scheduling and background execution for a production-grade mobile alarm clock.

## 7. Run instructions
Follow `README.md`: run `python -m http.server 8000` from the project directory and open `http://localhost:8000`. For a public demo, deploy the folder to a static HTTPS host such as GitHub Pages.

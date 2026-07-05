# Contribution [1]: [Feature] Clock with seconds

**Contribution Number:** [1]  
**Student:** [Samuel Perez]  
**Issue:** [https://github.com/nightscout/cgm-remote-monitor/issues/8048]
**Status:** [Phase III]

Nightscout currently lacks the ability to display seconds in the clock widget. A feature request was opened to add a SHOW_SECONDS environment variable as a browser-level setting, which would let users opt into showing seconds without affecting anyone who doesn't want it. A previous contributor submitted PR #8392 attempting this feature, but it was not merged due to four specific issues identified by the maintainer.
---

## Why I Chose This Issue

I chose this issue because the maintainer already reviewed and approved the overall direction of the feature, leaving a precise and actionable list of what needs to be corrected before it can be merged. Rather than guessing what to build or how to structure it, the feedback from the maintainer acts as a clear checklist.
---

## Understanding the Issue

### Problem Description

I need to add a feature that add seconds in a clock that has minutes and hours

### Expected Behavior

Display seconds live 

### Current Behavior

No seconds so it is a non-existent feature

### Affected Components

server route -> clocks.js
main template -> clock.html
config UI -> clock-config.html
Shared styles -> clock-shared.css
Config Styles -> clock-config.css
Client logic -> clock-client.js
Bundle entry -> bundle.clocks.source.js

---

## Reproduction Process

### Environment Setup

Cloned my fork, created a Python-free Node environment (Node LTS v22), `npm install`, and set
up `my.env` with a MongoDB connection, `API_SECRET`, `NODE_ENV=development`, and
`INSECURE_USE_HTTP=true`. Ran the app with `npm run dev` (env-cmd + nodemon).

The main setup challenge came later during verification: understanding that in
`NODE_ENV=development` the client JS is served **in-memory** by webpack-dev-middleware from
`/devbundle`, not from the on-disk `/bundle` directory (see Challenges).

### Steps to Reproduce

1. Run Nightscout with the default settings.
2. Observe the dashboard clock top-left.
3. **Observed result:** it shows `HH:MM` only (e.g. `23:35`), never seconds.

### Reproduction Evidence

- **My findings:** `grep -rn` for `SHOW_SECONDS` / `showSeconds` returned zero matches in the
  codebase — the feature genuinely did not exist. The format constants in
  `lib/client/index.js` (`FORMAT_TIME_12 = '%-I:%M %p'`, `FORMAT_TIME_24 = '%H:%M%'`) had no
  seconds token, and `updateClock` intentionally ticked at `min(15s, time-to-next-minute)`.

---

## Solution Approach

### Analysis

Root cause: there was no seconds-capable time format and no per-user setting to select it, plus
the clock refresh interval was minute-aligned (~15s max) so even a seconds format would not tick
live. PR #8392 took the right approach but shipped four defects flagged by the maintainer.

### Proposed Solution

Register a new `showSeconds` browser setting (auto-exposed as the `SHOW_SECONDS` env var),
render a seconds time format when it is enabled, and drop the clock tick to 1s in that mode.
Wire it through the settings dialog and the Azure deploy template, and fix the four defects.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** Add an opt-in `SHOW_SECONDS` browser setting that makes the dashboard clock
display live seconds, without regressing default behavior.

**Match:** Mirrored the existing boolean browser settings `nightMode` / `editMode` end to end —
default in `lib/settings.js`, a `mapTruthy` value mapper, load/save in
`lib/client/browser-settings.js`, a checkbox in `views/index.html`, and a param+appSettings
pair in `azuredeploy.json` (modeled on `NIGHT_MODE`). Nightscout auto-derives the env-var name
via `_.snakeCase(key).toUpperCase()`, so `showSeconds` → `SHOW_SECONDS` for free.

**Plan:** 
1. `lib/settings.js` — add `showSeconds: false` default + `showSeconds: mapTruthy` mapper.
2. `lib/client/index.js` — add `FORMAT_TIME_12_SECONDS` / `FORMAT_TIME_24_SECONDS`, use them in
   `getTimeFormat()`, and make `updateClock()` tick at 1s when enabled.
3. `lib/client/browser-settings.js` — load + persist the checkbox.
4. `views/index.html` — add the "Show Seconds" toggle to the settings dialog.
5. `tests/settings.test.js` — add `SHOW_SECONDS` and update the expected env-var count.
6. `azuredeploy.json` — add the `show_seconds` parameter and wire it into `appSettings`.

**Implement:** Branch `feat/clock-seconds-display` on my fork

**Review:** eslint clean, `git diff --check` clean, JSON validates, tests pass, diff scoped to
the 6 intended files (reverted an unrelated whitespace-only edit to `clock-client.js`).

**Evaluate:** Validate locally by `npm run dev ` and check the timer in the dashboard to confirm the "seconds" feature are working as expected.

---

## Testing Strategy

### Unit Tests

- [x] `tests/settings.test.js` "support setting from env vars" — added `SHOW_SECONDS` to the
      expected list and updated the hard-coded count from 24 to 25. Suite passes: **13 passing**.
- [x] Verified the four `getTimeFormat` branches produce correct output against the real d3
      library: `12h/off → 2:05`, `12h/on → 2:05:09 PM`, `24h/off → 14:05`, `24h/on → 14:05:09`.
- [x] Verified `updateClock` interval logic: 1000ms when `showSeconds` is on, 15000ms otherwise.

### Integration Tests

- [x] `npx eslint lib/client/index.js lib/client/browser-settings.js lib/settings.js
      tests/settings.test.js` — clean (confirms the `no-redeclare` fix).
- [x] `git diff --check` — clean (confirms the trailing-whitespace fix).
- [x] `azuredeploy.json` parses as valid JSON and `SHOW_SECONDS` is wired into
      `siteConfig.appSettings`.

### Manual Testing

Ran the app locally (`NODE_ENV=development`, `TIME_FORMAT=24`, `SHOW_SECONDS=true`). After
restarting the dev server and reloading, the dashboard clock rendered live seconds and ticked
every second. Also validated the UI path: hamburger → Settings → "Show Seconds" → Save.

---

## Implementation Notes

**What I built:** An opt-in `SHOW_SECONDS` browser setting that adds live seconds to the
dashboard clock (both 12h and 24h), a settings-dialog checkbox to toggle it, Azure one-click
deploy support, and fixes for all four maintainer-identified defects from PR #8392:

1. `no-redeclare` lint error — declared `var interval` once and assigned it in each branch of
   `updateClock()`.
2. Failing settings test — added `SHOW_SECONDS` and bumped the expected count 24 → 25.
3. Azure template — the `show_seconds` parameter is now actually wired into
   `siteConfig.appSettings` as `SHOW_SECONDS` (previously declared but non-functional).
4. Trailing whitespace in `browser-settings.js` — removed.

I re-implemented the feature cleanly rather than building on the prior branch, so the four
defects are avoided by construction.

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]


# Clockify Time Tracker — Changelog

**Date:** 07 April 2026

---

## Before You Start

If you have used a previous version of this tracker, do a hard reload before opening the updated file:

| OS | Shortcut |
|---|---|
| Windows / Linux | `Ctrl + Shift + R` |
| macOS | `Cmd + Shift + R` |

Or manually: **Chrome menu → More Tools → Clear browsing data → Cached images and files**, then reopen the file.

This clears any stale JavaScript the browser has cached. Your saved settings (API key, workspace, idle project, preferences) are stored in `localStorage` and will **not** be affected.

---

## Fix: Idle Notification

Replaced the previous notification stack (browser Notification API + blocking `alert()` dialog + flashing tab title) with a fully `file://`-compatible approach that requires zero permissions.

**Previous behaviour:**
- Attempted to fire a desktop `Notification` — silently blocked by Chrome when opened as a local file
- Fell back to a blocking `alert()` dialog that froze the page until dismissed
- Flashing tab title ran independently and had to be stopped separately

**New behaviour:**
- **Triple beep** — three short sine-wave tones generated via the Web Audio API, no audio file required
- **Flashing tab title** — alternates between `⚠ IDLE — Clockify` and the normal title every second
- **Amber banner** — slides in below the clock-in bar with the configured idle threshold and a Dismiss button

**Reset conditions — all three clear automatically when:**
- A task is started
- A task is stopped or discarded (countdown restarts fresh)
- You clock out
- You manually dismiss the banner (banner + tab flash only)

**Removed:**
- `Notification` API calls entirely
- `requestNotifPermission()` function
- `updateNotifStatus()` function
- Enable Notifications button from Settings
- Permission status note from Settings

---

## Fix: Manual Entry Duration Computation

The manual entry form now has an explicit **min / hrs toggle** so the duration field is unambiguous regardless of what you type.

**Previous behaviour:**
- Single input accepted multiple formats (`1.25`, `1:30`, `90mins`) with internal parsing logic — prone to misinterpretation

**New behaviour:**
- **min mode** — enter plain minutes, e.g. `90` = 1h 30m
- **hrs mode** — enter decimal hours, e.g. `1.5` = 1h 30m
- Mode persisted to `localStorage` (`cfy_dur_mode`) so your preference survives page reloads
- Live preview shows the parsed result as you type: `→ 90 min · 1.50h (1h 30m)`
- On blur, the field normalises itself back to the rounded value in the active mode
- Both modes round to the nearest 5 minutes before pushing to Clockify

---

## Added: Task Description Timing Preference

A new **Before / After** toggle in Settings controls when the description prompt appears.

**Before (prompt on start):**
- Clicking a task button opens the description modal immediately, before the timer starts
- You describe what you are about to work on
- The description is passed into `startTimer()` and pre-fills the live description field
- Modal confirm button reads `Start Timer →`

**After (prompt on stop) — default:**
- Original behaviour preserved
- Clicking Stop opens the modal with the task name, duration, and rounded time
- The live description field value is pre-filled into the modal so anything typed mid-task carries over

**Persistence:** Saved to `localStorage` (`cfy_desc_timing`). Displayed and changeable in Settings under the Clock-In Project card.

---

## Added: Live Task Description — Inline Edit and Capture

While a task timer is running, a text field appears below the task name in the live bar so you can jot notes mid-task without waiting for the stop prompt.

**Behaviour:**
- Field becomes visible as soon as a task starts, hidden when idle
- Placeholder reads `What are you working on...` (After mode) or `Add more detail anytime...` (Before mode)
- Value is carried forward into the stop modal automatically — no retyping needed
- If the timer is restored from `localStorage` on page reload, the field reappears with the restore placeholder
- Field clears when the timer is cleared (`clearTimer()`)
- Pressing Enter in the stop modal textarea confirms and pushes — consistent with existing keyboard shortcut

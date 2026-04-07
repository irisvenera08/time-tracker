# Clockify Time Tracker — v1.1.2

**Date:** 08 April 2026

---

## Added: Description Autocomplete

While typing a description (in the live bar or stop modal), a dropdown suggests matching descriptions you've already used today. Click or keyboard-navigate to populate the field instantly — no retyping the same memo twice.

- Matches on any word in any order — `debug case` finds `Debug: case98204 problem with invoicing`
- Matched words highlighted in the dropdown
- Keyboard: `↑ ↓` to navigate, `Enter` or `Tab` to select, `Esc` to dismiss
- Cache updates immediately after each push — available for the next entry without a reload
- Works in both **Before** and **After** description timing modes

---

## Fix: Autocomplete Fetch Optimisation

The initial implementation fetched today's entries with `hydrated=true` and `page-size=200`, pulling full project/task objects that were never used.

**Now:**
- `hydrated=true` removed — only `e.description` is needed
- `page-size=50` — sufficient for a day's unique descriptions
- Date range uses **local midnight**, not UTC — correct across all timezones

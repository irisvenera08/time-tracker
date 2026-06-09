# Clockify Time Tracker — Changelog

## v1.1.3 — 09 June 2026

---

## Before You Start

If you have used a previous version of this tracker, do a hard reload before opening the updated file:

| OS | Shortcut |
|---|---|
| Windows / Linux | `Ctrl + Shift + R` |
| macOS | `Cmd + Shift + R` |

Or manually: **Chrome menu → More Tools → Clear browsing data → Cached images and files**, then reopen the file.

Your saved settings (API key, workspace, idle project, preferences) are stored in `localStorage` and will **not** be affected.

---

## Added: Project Sorting Preference

A new **Project Sorting** dropdown in **Settings → Clock-In Project** controls the order of projects everywhere they appear — the Track tab, the Manual Entry project dropdown, and the Idle Project dropdown.

**Options:**
- **Alphabetical — Project Name** (default) — sorts by the *displayed* project name, with the `PRJ` prefix stripped. Previous versions sorted on the raw name including the prefix, so ordering may shift slightly.
- **Alphabetical — Project ID** — sorts by the `PRJ` code, numeric-aware (`PRJ2` < `PRJ10` < `PRJ100`). Projects without a PRJ code sort to the end, alphabetically.
- **Clockify order** — the exact order the Clockify API returns projects in.

**Details:**
- Sorting applies live during progressive project loading, not just after load completes
- Changing the setting re-sorts immediately — no reload needed
- Persisted to `localStorage` (`cfy_sort`) so your preference survives page reloads

---

## Fix: Save & Push No Longer Loses Time Entries

Previously, **any** failed push silently destroyed the time entry: the modal closed, a `Clockify push failed` toast appeared, and the timer was cleared — the tracked time was gone. This affected empty descriptions (in workspaces with *"Do not allow saving without description"* enabled), network drops, expired API keys, and rate limits alike.

**New behaviour — empty description:**
- Clicking **Save & Push** with an empty memo no longer fires an API call
- The modal stays open, the textarea border turns red, and the placeholder reads `⚠ Description required — type a short note to push`
- A toast confirms the entry is still held
- Warning styling clears as soon as you start typing

**New behaviour — push failure (any cause):**
- Timer state, the pending entry, and the rounded duration are all preserved
- The modal stays open with the title `⚠ Push failed — entry not lost` and the confirm button changes to `Retry Push →`
- The modal subtitle shows the project, task, duration, and a hint about what to check (connection, API key, missing description)
- Fix the issue and retry — nothing is ever discarded automatically
- **Cancel** returns you to the still-running timer; **Discard** remains the only way to intentionally throw time away

**New behaviour — clock-out:**
- Clock-out pushes the final entry silently with an empty memo. In workspaces requiring descriptions, this previously failed every time and ate the entry.
- Now a failed clock-out push opens the modal, **pauses the clock-out**, and notes: push first, then clock out again

**Also:**
- The confirm button disables and shows `Pushing...` during the API call, preventing accidental double-pushes
- The autocomplete dropdown now closes properly when the modal closes after a successful push

---

## localStorage Keys

| Key | Added in | Purpose |
|---|---|---|
| `cfy_sort` | **v1.1.3** | Project sorting preference (`name` / `id` / `clockify`) |

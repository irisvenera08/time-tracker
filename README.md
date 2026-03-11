# ⏱ Clockify Tracker

A fast, keyboard-friendly time tracker that pushes directly to [Clockify](https://clockify.me) — built for people who find Clockify's own UI too slow for daily use.

**No install. No server. No account needed beyond Clockify.**
Just open the HTML file in any browser and go.

![ClockifyTracker](https://img.shields.io/badge/Clockify-API%20Powered-blue) ![License](https://img.shields.io/badge/license-MIT-green) ![No Install](https://img.shields.io/badge/install-none-brightgreen)

---

## ✨ Features

- **One-click task tracking** — projects and tasks loaded from your Clockify workspace, grouped and color-coded
- **Clock In / Clock Out** — tracks your whole workday, auto-starts an idle/overhead entry in Clockify when you clock in
- **Auto-switch tasks** — click a new task while one is running; it stops the current one and starts the new one seamlessly
- **Discard** — made a mistake? Stop the timer without pushing anything to Clockify
- **Manual entry** — log time after the fact with flexible duration input (`1.25`, `1:30`, `90` mins)
- **5-minute rounding** — durations round to the nearest 5 mins (22m → 20m, 23m → 25m), applied correctly to the Clockify payload
- **Log tab** — pulls your real Clockify entries for any date range, with total hours and CSV export
- **Idle alert** — JS popup + flashing tab title if no task has been running for 30+ minutes (configurable)
- **Project search** — search bar filters projects and tasks instantly
- **Sticky top bar** — clock-in status and live timer stay visible as you scroll
- **Works everywhere** — desktop Chrome/Firefox/Safari, iPad, iPhone, Android

---

## 🚀 Getting Started

### Option A — Use directly (no hosting needed)
1. Download `index.html`
2. Open it in your browser
3. Go to **Settings**, paste your Clockify API key, click **Save & Connect**

### Option B — Host on GitHub Pages (recommended)
1. Fork this repo
2. Go to **Settings → Pages → Branch: main → Save**
3. Your tracker lives at `https://yourusername.github.io/clockify-tracker`
4. Bookmark it or add to your home screen on mobile

---

## 🔑 Getting Your Clockify API Key

1. Log in to [clockify.me](https://clockify.me)
2. Go to **Profile Settings** (top right avatar)
3. Scroll to **API** at the bottom
4. Click **Generate** — copy the key
5. Paste it into the tracker's Settings tab

Your API key is stored only in **your browser's localStorage** — it never leaves your device except to talk directly to Clockify's API.

---

## ⚙️ First-Time Setup

After connecting, go to **Settings → Clock-In Project** to configure:

| Setting | Description |
|---|---|
| **Idle / Overhead Project** | The Clockify project that runs automatically when you clock in (e.g. "Admin", "Overhead") |
| **Idle Task** | The task within that project |
| **Idle Alert (minutes)** | How long before the idle popup fires (default: 30 mins) |

---

## 🎯 How It Works

```
Clock In
  └─ Starts idle/overhead entry in Clockify

Click a task button
  └─ Stops idle entry → starts task entry in Clockify
  └─ Live HH:MM:SS timer shown at top

Stop or switch task
  └─ Modal appears: add a memo, then Save & Push or Discard
  └─ Duration rounded to nearest 5 mins before pushing
  └─ Switching tasks: stops current, starts next seamlessly

Clock Out
  └─ Stops everything, closes the day
```

---

## 📋 CSV Export

The Log tab exports a CSV compatible with NetSuite timesheet imports:

```
Date, Duration, Customer:Job, Task, Memo, Project Phase
```

---

## 🔒 Privacy & Security

- **No backend, no database, no analytics**
- Your API key is stored only in your own browser (`localStorage`)
- All API calls go directly from your browser to `api.clockify.me`
- Nothing is ever sent to any third-party server

---

## 🛠 Tech Stack

Plain HTML + CSS + vanilla JavaScript. Single file. Zero dependencies. Zero build steps.

---

## 🤝 Contributing

PRs welcome! Some ideas for future improvements:

- [ ] Tags support
- [ ] Weekly summary view
- [ ] Dark/light theme toggle
- [ ] Pomodoro timer mode
- [ ] Multiple simultaneous workspace views

---

## 📄 License

MIT — free to use, modify, and distribute.

---

*Built with ❤️ for consultants, freelancers, and anyone who logs time for a living.*

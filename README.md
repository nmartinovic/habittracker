# habittracker# Everyday‑style Habit Tracker

A single‑file, **GitHub Pages‑friendly** habit tracker inspired by the look and feel of [everyday.app](https://everyday.app/). It stores data **locally** (in your browser’s `localStorage`), shows **all habits at a glance** in a horizontal chain view, and supports **Done / Failed / Skipped** states with **streak‑based darkening** and **weekend continuity** when auto‑skip is enabled.

> **Status:** Lightweight MVP, optimized for solo use. No server, no build step, just `index.html`.

---

## Features

* **Chain view (default):** All habits visible in rows; recent days displayed in columns.
* **Local‑only data:** No accounts, no sync. Everything lives in your browser `localStorage`.
* **States:** `Done`, `Failed`, `Skipped`. Click a cell to cycle through states (Pending → Done → Failed → Skipped → Clear).
* **Auto‑skip weekends:** When enabled on a habit, Sat/Sun don’t break your streak.

  * **Weekend continuity tint:** Weekends visually carry **Friday’s shade** so the chain doesn’t visually break.
* **Streak shading:** Each habit has its own color; **Done** cells get **progressively darker** as the streak grows (4 levels).
* **Per‑habit stats:** `🔥 Current`, `⭐ Longest`, `✓ Total` (longest is computed across all recorded days; current walks back from **today**, letting `Skipped` pass through).
* **Inline Today status:** A pill in each row shows Today’s status (click to cycle).
* **Light/Dark theme:** Toggle in the toolbar.
* **Windowed navigation:** Fixed **15‑day** window; use ◀ / ▶ to move one day (**Shift** to move 7 days). **Today** snaps back.
* **Modal editing:** Click a habit name or the `⋯` button to rename, change color, toggle auto‑skip, or delete the habit.
* **Backup/Restore:** Export/Import JSON from the toolbar.

---

## Quick Start (GitHub Pages)

1. Create a new GitHub repo (public or private).
2. Add the provided **`index.html`** to the repo root and commit.
3. In **Settings → Pages**, choose **Deploy from a branch**, then **main** and **/** (root). Save.
4. Wait \~1–2 minutes; open the Pages URL GitHub shows.

No build or tooling required.

---

## Usage

* **Add a habit:** `＋ New Habit` (toolbar).
* **Mark a day:** Click any cell to cycle states.
* **Today status:** Click the `Today: …` pill in a habit row to cycle just today.
* **Edit a habit:** Click the habit name or `⋯` → modal opens (Name, Color, Auto‑skip, Delete).
* **Navigate dates:** Use **◀ / ▶** to shift the 15‑day window by 1 day; hold **Shift** for ±7 days; **Today** snaps the window to end on today.
* **Theme:** `Light ↔ Dark` in the toolbar.
* **Backup/Restore:** `Export` (downloads JSON) / `Import` (select your saved JSON).

**Keyboard shortcuts**

* **← / →**: shift date window (hold **Shift** for ±7 days).

---

## How streaks work

* **Current streak**: Count consecutive `Done` days walking **backward from today**. `Skipped` days do **not** break the streak; `Failed` and **past** `Pending` days break it.
* **Longest streak**: Scan from the **first recorded day** to the **last recorded day**, counting the longest run of `Done` days (with `Skipped` allowed).
* **Weekend continuity**: If **Auto‑skip weekends** is enabled, Saturday/Sunday are tinted with **Friday’s shade** (visual only; they don’t advance the count).

---

## Data & Backup

Data lives in your browser under the key `habitTracker_v1`.

**Export format (example, trimmed):**

```json
{
  "selectedHabitId": null,
  "habits": [
    {
      "id": "9d5a…",
      "name": "Daily Focus",
      "color": "#6b8cff",
      "createdAt": "2025-09-01",
      "autoSkipWeekends": true,
      "entries": {
        "2025-09-01": "done",
        "2025-09-02": "skipped",
        "2025-09-03": "failed"
      }
    }
  ],
  "ui": {
    "windowDays": 15,
    "windowEndISO": "2025-09-04",
    "theme": "light"
  }
}
```

* `entries` maps `YYYY-MM-DD` → one of `"done" | "failed" | "skipped"` (absent = pending).
* `ui.windowDays` is fixed at 15 by default; adjust in code if desired (see **Customization**).

> **Tip:** Back up before clearing browser storage or switching devices.

---

## Customization

You can tweak a few simple defaults directly in **`index.html`**:

* **Default window size** (15 days):

  ```js
  const defaultState = {
    // …
    ui: { windowDays: 15, windowEndISO: fmt(today), theme: 'light' }
  };
  ```
* **Default theme**: change `theme: 'light'` to `'dark'`.
* **Habit palette**: the color `<select>` in the modal defines available options; add your favorite hex colors there.
* **Streak darkening curve**: the function `shadeForStreak(baseHex, theme, level)` controls how quickly cells darken as a streak grows (4 levels). Adjust the weight arrays for a stronger/softer effect.

---

## Troubleshooting

* **I don’t see my data anymore**: Ensure you didn’t clear site data or use a different browser/profile. Restore from your last export if needed.
* **No horizontal page scroll**: That’s by design—only the date **window** moves with the arrows. If you see page‑level horizontal scroll, ensure you haven’t changed the base CSS (the body uses `overflow-x: hidden`).
* **New habits look far from the header**: Spacing is intentionally tight; if you customized CSS, keep `row-gap` small in `.chain` (e.g., `4px`).
* **Mobile tap targets**: Increase `--cell-size` in `:root` for bigger cells (and possibly `--grid-gap`).

---

## Privacy

* 100% client‑side. No analytics, no network requests.
* You control backups via Export/Import.

---

## Roadmap (optional ideas)

* Month view toggle (calendar grid)
* CSV export per habit
* PWA (offline install + icon)
* Reminder notifications
* Per‑habit targets (e.g., 5x/week)
* Grace period setting for current streak (e.g., ignore breaks ≤ N days)

---

## License

Use however you like for personal projects. If you plan to share/distribute, consider adding an MIT License.

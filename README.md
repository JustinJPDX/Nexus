# Project Nexus

**Measurement Dashboard** for a VMware migration demand generation program.

Single-file HTML dashboard hosted on GitHub Pages. No build step, no dependencies, no server. Update the JSON data block, push to main, and the live dashboard reflects the changes.

## Live URL

`https://nexusreporting.github.io/Nexus/`

Password protected. Contact the project lead for access.

---

## What it does

Tracks the full lifecycle of a demand generation pilot: lead sourcing through content syndication, marketing automation and qualification, AI outbound calling, and partner opportunity delivery. Designed for a mixed audience of program operators, functional leadership, and external stakeholders.

**11 tabs:**

- **Summary** — KPI tiles, weekly volume trend, pickup rate, open measurement gaps
- **Targets and Pace** — Quarterly PO burndown, velocity tracking, pipeline feed vs target, revenue model
- **Funnel and Age** — Lifecycle stage counts, stage dwell time, attrition rates, qualification gates
- **AI Calls (NLPearl)** — Attempt and connect volume, pickup rate, call signal distribution, SLA compliance, benchmarks
- **Campaign Reporting** — Audience intelligence: job function, revenue size, employee count, industry, VM environment, migration timeline, decision role, geographic distribution, campaign pacing
- **Email** — Consent and link testing status, benchmarks
- **Web** — Analytics tagging status, implementation checklist, benchmarks
- **Improvement Plan** — Governance cadence and CIP structure
- **Lead Scoring** — Fit vs activity scoring matrix, weight configuration, decay rules
- **Metric Register** — Full metric catalog with MET IDs, calculations, sources, and priority tiers
- **Glossary** — Searchable reference for all program terms, acronyms, and systems

---

## Features

- **Password gate** — Prompt on page load. Session-persisted so refresh does not re-prompt.
- **Light and dark theme** — Toggle in the topbar. Defaults to light.
- **Edit mode** — Inline field editing with changelog tracking. Save and Export downloads a new versioned HTML file with updated data baked in.
- **Collapsible insight notes** — 23 analyst notes throughout the dashboard, collapsed by default. Hidden on mobile.
- **Chart tooltips** — Hover any canvas chart to see exact data point values.
- **Print support** — Print Tab button in the footer. Sidebar hidden, active tab expands to full width. Notes auto-expand in print.
- **Mobile responsive** — Hamburger nav, single-column reflow, adaptive typography below 768px.
- **Persistent data footer** — Fixed bar showing data-as-of date, version number, and print button.
- **Search** — Topbar search filters sidebar nav by keyword across all tabs.
- **Glossary search** — Dedicated search within the glossary tab.

---

## How to update the dashboard

All data lives in a single JSON block at the top of `index.html`. You do not need to be a developer to update it. If you can open a file in a text editor and change a number, you can update the dashboard.

### What you need

- A plain text editor. In order of preference:
  - **VS Code** (free at code.visualstudio.com) — recommended
  - **Notepad** (Windows, built-in) — works fine
  - **TextEdit** (Mac, built-in) — must be in plain text mode
- Do not use Microsoft Word. It will corrupt the file.

### Step-by-step

**1. Download the file**

Go to the repository on GitHub, click `index.html`, then click the download icon (or use `Code > Download ZIP`). Save it to your desktop.

**2. Open in a text editor**

Open the file in VS Code or Notepad. Do not open it in Word or any rich-text editor.

**3. Find the data block**

At the very top of the file, search for:

```
NEXUS DASHBOARD - DATA BLOCK
```

Use `Ctrl+F` (Windows) or `Cmd+F` (Mac) to jump straight to it. This is the only section you will edit. Everything below it is layout and logic — do not touch it.

**4. Update the values**

Each field has a comment explaining what it is and where it appears. For example:

```json
"q1_delivered": 0,          ← update when POs are confirmed in ACE
"pickup_rate_pct": 24,      ← from the weekly NLPearl export
"total_in_funnel": 264,     ← from Eloqua pipeline pull
```

Change only the value after the colon. Do not change the field name, the quotes, or the comma at the end of the line.

**5. Save the file**

`Ctrl+S` on Windows, `Cmd+S` on Mac.

**6. Upload to GitHub**

Go to the repository, click `Add file > Upload files`, drag in your updated `index.html`, and commit directly to `main`. GitHub Pages redeploys automatically, usually within 2 minutes.

**7. Verify**

Open `https://nexusreporting.github.io/Nexus/` and confirm the numbers look correct before sharing.

---

### Alternatively: use Edit Mode

If you prefer not to touch the file directly, use Edit Mode in the live dashboard:

1. Open the dashboard and enter the password
2. Click **Edit Mode** in the topbar
3. Click any data field to edit it inline
4. When done, click **Save and Export** — this downloads a new versioned HTML file with all changes baked in
5. Upload that file to GitHub as `index.html`

---

### Which fields to update and who owns them

| Field | Owner | When |
|---|---|---|
| Funnel counts (Known, Validation, MEL, PO, Recycled, DQ) | Justin / Allison | Weekly, after Eloqua pipeline pull |
| PO delivered (`q1_delivered`, `total_delivered`) | Justin / John | As soon as ACE uploads are confirmed |
| Weeks remaining (`q1_weeks_remaining`) | Justin / John | Weekly |
| NLPearl call stats (attempts, pickup rate, signals) | Justin / Daniel | Weekly, from NLPearl export |
| Trend chart arrays (`mel`, `mqp`, `po`, `rates`) | Justin / John | Weekly — append one value per array |
| Campaign totals (`total_leads`, `spend_to_date`) | Justin | When new batch arrives |
| Open gaps text (`open_gaps_text`) | Justin | When open items change status |
| Summary hero subtitle | Justin | Weekly or when key numbers shift |
| Report date and as-of label | Justin | Each update cycle |

Fields not listed (audience intelligence tables, geographic data, industry breakdowns) are updated only when a new content syndication batch arrives. Those require a full batch regeneration rather than a manual edit.

---

### Common mistakes to avoid

**Editing the wrong part of the file**
The data block is at the very top. If you are scrolling through hundreds of lines of code, you have gone too far. Use `Ctrl+F` to search for `NEXUS DASHBOARD - DATA BLOCK`.

**Breaking the JSON format**
Each line follows this pattern:
```
"field_name": value,
```
Common errors that will break the dashboard:
- Deleting a comma at the end of a line
- Deleting a quotation mark around a text value
- Changing a field name instead of its value
- Using a comma inside a number (write `79900`, not `79,900`)

If the dashboard opens as a blank page or shows no data after uploading, the JSON is likely broken. Re-download the previous version from the repository and start over from there.

**Opening the file in Word**
Word will corrupt the HTML. Always use a plain text editor. If you accidentally open in Word, close without saving and use the GitHub copy.

---

## Architecture

Single self-contained HTML file. No external JS libraries. No build tooling.

- **CSS custom properties** for theming (light/dark token swap on `[data-theme]`)
- **Vanilla JS** for navigation, chart rendering, search, edit mode, and tooltips
- **Canvas 2D** for all charts (trend, pickup, age, calls, burndown, velocity, cumulative, VM size, timeline, states)
- **Google Fonts CDN** for DM Sans, DM Mono, and Bebas Neue — the only external dependency

## File structure

```
index.html    # Dashboard (single file, everything included)
README.md     # This file
```

## Requirements

A modern browser. Chrome, Firefox, Safari, or Edge. No IE support.

GitHub Pages configured to deploy from `main` branch, root directory.

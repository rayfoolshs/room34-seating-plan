# Room 34 — Seating Plan Builder

A single-file, offline web app that rebuilds the **2027 Room 34 Seating Plan**
template and lets you fill it in live, then print or save it as a PDF.

Open **`index.html`** in any browser (double-click it). No install, no server,
no internet connection required.

Current version: **1.0.0** (see [`VERSION`](VERSION) and [`CHANGELOG.md`](CHANGELOG.md)).

---

## Features

- **Faithful template** — the room outline, seat boxes, header and Chromebook
  list are reproduced as scalable SVG directly from the original PDF, so the
  output matches the source template.
- **Live editing** — type a name into any field and it appears on the plan
  instantly. Long names automatically shrink and wrap to fit their box.
- **Year group header** — an input whose value is shown after the header, e.g.
  `Seating Plan : Year 9`.
- **Seat positions** — top row (1–7) and right column (8–11).
- **Chromebook assignments** — `LT548 – LT559`.
- **CSV import** — load a class list with `Name` and `Surname` columns and
  populate fields in one step.
- **Print / Save as PDF** — prints just the plan in landscape.
- **Autosave** — names are saved in the browser's local storage.
- **Reset** — clears all names and the year group.

## Usage

1. Double-click `index.html`.
2. Type the **Year group** (e.g. `9`) — it appears after *Seating Plan :*.
3. Type names into the fields, **or** use **Import class list (CSV)**.
4. Click **Print / Save as PDF** when done.

### Importing a CSV

Upload a CSV with **Name** and **Surname** columns. A header row is optional.
The importer also understands `First name` / `Last name`, a single `Full name`
column, and `;` or tab separated files.

```csv
Name,Surname
Alex,Smith
Priya,Patel
Jordan,Lee
```

A **Download sample CSV** link is provided in the app. The placement dropdown
controls where imported names go:

| Option | Fills |
| --- | --- |
| Fill seats, then Chromebooks | Seats 1–7, then 8–11, then LT548–LT559 |
| Fill seats only | Seats 1–7, then 8–11 |
| Fill Chromebooks only | LT548–LT559 |

## Files

| Path | Description |
| --- | --- |
| `index.html` | The web app (self-contained). |
| `templates/2027 Room 34 Seating Plan Template.pdf` | The original source template. |
| `PLAN.md` | How the app was designed and built, step by step. |
| `CHANGELOG.md` | Version history. |
| `VERSION` | Current version number. |

## Notes on the source template

- The original PDF prints **`LT550` twice** (13 rows). This app uses the correct
  `LT548 – LT559` sequence (12 rows), as requested.
- The original PDF has a **stray "7"** floating on the left wall. It was removed
  from this app.

## License

© rayfoolshs. All rights reserved.

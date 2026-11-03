# Build Plan — Room 34 Seating Plan Builder

This document records the plan used to build the app, and the reasoning behind
each decision. It doubles as a guide for extending the app later.

## Goal

Take the supplied `2027 Room 34 Seating Plan Template.pdf` and turn it into an
interactive, web-based seating plan where a teacher can:

1. Enter a **year group** that appears after the header *Seating Plan :*.
2. Type student names into each numbered position and see them appear on the
   template **live**.
3. Fill the **Chromebook assignments** (`LT548`–`LT559`) with student names.
4. **Import a class list from CSV** (`Name`, `Surname`) to populate fields at once.
5. **Print / Save as PDF** a finished, clean seating plan.

Constraints: keep it simple for a non-technical user — a single HTML file that
opens by double-click; no build step, no server, no internet.

## Step 1 — Reverse-engineer the template geometry

The model could not read the PDF directly, so the layout was extracted
programmatically with **PyMuPDF**:

- `page.get_text("dict")` gave every text span with its bounding box, font and
  size (the header, the seat numbers, the "Chromebook Assignments" heading and
  the `LT###` labels).
- `page.get_drawings()` gave every vector shape with its rectangle, fill and
  stroke (the room floor and all seat/furniture boxes).
- The page was rendered to PNG (`page.get_pixmap`) and inspected visually to
  confirm the result.

Key measurements (page is **720 × 540 pt**):

- Room floor: `(16.7, 53.2) → (694.6, 486.8)`, fill `#CFE2F3`, black stroke.
- Top-row seats 1–7: seven ~80.6 pt wide cells, `y = 53.2 … 102.8`.
- Right-column seats 8–11: four 49.6 × 80.6 cells at `x = 645 … 694.6`.
- Chromebook list starts at `x = 276.9`, first row at `y = 226.7`, step `16.8`.
- Header "Seating Plan :" baseline ≈ `y = 35.1`, size 14.
- Seat numbers are 7 pt, drawn ~2.8 pt right and ~9.6 pt down from each box
  top-left corner.

Two anomalies in the source were identified and flagged to the user:

- A **stray "7"** floating on the left wall (no box around it) → removed.
- **`LT550` duplicated** in the printed list (13 rows) → replaced with the
  correct `LT548–LT559` sequence (12 rows).

## Step 2 — Rebuild the template as scalable SVG

The app draws the whole template as an `<svg viewBox="0 0 720 540">`, using the
same coordinates as the PDF. This means:

- It scales to any screen or paper size with no loss of quality.
- It can be printed at A4/Letter landscape via `@page { size: landscape }`.
- Names are real `<text>` nodes that can be positioned and re-rendered on demand.

Boxes (seat cells and the unlabeled desk/strips) are drawn over the room floor;
seat numbers and the static labels are drawn on top; name text nodes are drawn
last so they always sit above the artwork.

## Step 3 — Live data binding

- Application state is a plain object: `{ year, seats: {id: name}, lts: {num: name} }`.
- Every input writes to state on `input` and calls a small render function for
  just that slot.
- `fitText()` measures candidate font sizes with a canvas `measureText` context
  and picks the largest size at which the name fits its box (wrapping to
  multiple lines if needed). Names never overflow their box.

## Step 4 — CSV import

- A hidden `<input type="file">` plus a styled label acts as the picker.
- `FileReader.readAsText` reads the file; a small hand-written CSV parser
  handles quoted fields and CRLF/LF line endings.
- The delimiter is auto-detected (`,` `;` or tab) from the first line.
- `extractNames()` recognises a header row (`Name`/`Surname`, `First`/`Last`,
  `Full Name`, single-column names, …) and otherwise assumes the first two
  columns are first name and surname.
- `applyImport()` clears the target slots and fills them in reading order. The
  destination is chosen with a dropdown: *seats then Chromebooks*, *seats only*,
  or *Chromebooks only*.

## Step 5 — Persistence, printing and polish

- State is saved to `localStorage` on every change and restored on load.
- **Print / Save as PDF** uses `window.print()` with a `@media print` stylesheet
  that hides the form and prints only the plan.
- A **Reset** button clears everything after confirmation.

## Step 6 — Testing

Validated by rendering the page in **headless Chrome** and screenshotting it:

1. Empty template compared against the original PDF render — geometry matched.
2. Sample names (including long ones) to verify auto-fit and wrapping.
3. Full CSV import through the real file picker path.

This surfaced and fixed a real bug: the field-sync routine originally selected
*all* inputs in the form, including the hidden file input, and threw when trying
to write a name into it. It now selects only `input[type="text"]`.

## Repository layout

```
index.html        the app
templates/        original source PDF
README.md         overview + usage
PLAN.md           this document
CHANGELOG.md      version history
VERSION           current version
```

## Future ideas (not yet implemented)

- Exact 13-row Chromebook layout to match the PDF literally.
- Surname-first display option (`Smith, Alex`).
- Editable seat labels / configurable number of seats.
- Export the filled plan back to PDF server-side, or a PNG download button.
- Save/load named multiple classes.

## Versioning

Semantic versioning. Each release is tagged (`v1.0.0`) and recorded in
`CHANGELOG.md`; the current number also lives in `VERSION`.

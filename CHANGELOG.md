# Changelog

All notable changes to this project are recorded here.
This project uses [semantic versioning](https://semver.org/).

## [1.0.0] — 2026-11-03

First release.

### Added
- Scalable SVG reproduction of the **2027 Room 34 Seating Plan** template,
  built from the original PDF geometry.
- Live name editing for seats **1–7** (top row) and **8–11** (right column).
- Live name editing for **Chromebook assignments LT548 – LT559**.
- **Year group** input, displayed after the header (e.g. `Seating Plan : Year 9`).
- Automatic text fit — long names shrink and wrap to fit their box.
- **CSV import** (`Name`, `Surname`; header optional) with placement modes:
  seats then Chromebooks, seats only, or Chromebooks only.
- **Download sample CSV** link.
- **Print / Save as PDF** (landscape) and browser **autosave**.
- **Reset** button.

### Changed / fixed
- Removed the stray "7" label floating on the left wall of the source template.
- Corrected the Chromebook list to `LT548 – LT559` (the PDF printed `LT550` twice).
- Fixed field synchronisation so it ignores the hidden file input.

[1.0.0]: https://github.com/rayfoolshs/room34-seating-plan/releases/tag/v1.0.0

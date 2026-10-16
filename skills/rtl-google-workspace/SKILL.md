---
name: rtl-google-workspace
description: Write Hebrew, Arabic or other right-to-left content into Google Docs, Sheets, Slides and Drive so it actually displays right-to-left — paragraph direction in Docs and Slides, RTL sheets and cells in Sheets, and Hebrew file and folder names in Drive. Use when creating or editing a Google Doc, Sheet or Slides deck with RTL text, or naming files and folders in Hebrew.
---

# RTL in Google Docs, Sheets, Slides and Drive

Apply the rules in the rtl-basics skill. This skill adds the Google-specific
switches. Field names come from Google's API reference (discovery documents read on
2026-10-16). Your tool may expose them under other names, or not at all. Check what
it offers.

## Google Docs

- **Paragraph direction defaults to left-to-right and is not inherited.** Google's
  reference says so for `ParagraphStyle.direction`: "If unset, the value defaults
  to LEFT_TO_RIGHT since paragraph direction is not inherited." Inserting Hebrew
  text does not change it. Every paragraph you write in Hebrew needs `direction:
  RIGHT_TO_LEFT`, set through a paragraph-style update over that paragraph's range,
  with `direction` in the update mask.
- **Alignment:** use `START`, not `END` or `RIGHT`. `START` follows the paragraph's
  direction, so it lands on the right for an RTL paragraph.
- **Whole-document RTL:** a section can also be set RTL (`SectionStyle.contentDirection`),
  which controls column order. That changes the layout, not each paragraph's
  direction, so paragraphs still need their own.
- **Order of operations:** insert all the text first, then apply the direction to
  the ranges in one batch. Text inserted after styling picks up whatever style is
  at the insertion point.
- **Tables:** set direction on the paragraphs inside each cell. Isolate
  multi-part numbers by giving them a cell of their own.
- **Creating from HTML or Markdown:** if your tool creates a Doc by converting
  HTML, put `dir="rtl"` on every block in the HTML. Markdown has no way to express
  direction, so a Doc made from Markdown comes out LTR and needs fixing afterwards.
- **Verify:** read the document structure back and check the paragraphs' direction.

## Google Sheets

- **RTL sheet:** set the sheet property `rightToLeft: true`. Column A moves to
  the right and the grid reads right-to-left. Do this for any sheet whose content
  is mainly Hebrew. It is a per-tab property, so set it on each tab.
- **Cells:** `CellFormat.textDirection: RIGHT_TO_LEFT` and right alignment for
  Hebrew text cells in a sheet that is otherwise LTR.
- **Locale and time zone:** for an Israeli household, set the spreadsheet locale
  to Hebrew/Israel and the time zone to `Asia/Jerusalem`, so dates parse day-first
  and currency formats show ₪. Write dates as real date values, not text.
- **Currency format:** a number format pattern like `#,##0.00 [$₪-he-IL]`, or
  simply `#,##0.00 ₪`.
- Phone and ID numbers: store as text (a leading `'`, or a text number format) so
  leading zeros survive. `050…` stored as a number becomes `50…`.

## Google Slides

- Paragraph `direction: RIGHT_TO_LEFT` on each RTL paragraph in text boxes and
  shapes. As in Docs, it is not inherited.
- Right-align, and choose a theme font that covers Hebrew (Arial, Rubik, Assistant,
  Heebo and Noto Sans Hebrew all do).

## Drive: file and folder names

- Hebrew file and folder names are fine. Keep the extension in ASCII (`.pdf`).
- **Do not put U+200F or other invisible direction marks in names.** They break
  search, sorting and exact-name lookups later, and nobody can see why.
- Put a date in ISO form (`2026-10-16`) at the **start** of the name, then the
  Hebrew: `2026-10-16 חשבון ארנונה.pdf`. Names sort by date in every interface,
  and the date displays correctly whatever the interface language.
- When you search for a file by its Hebrew name, also try variants with and
  without the final-letter forms (ך ם ן ף ץ) and gershayim (`ש"ח` / `שח`). Names
  typed by people are inconsistent.

## Uploading files

A .docx, .pdf or .xlsx uploaded to Drive keeps the direction it was made with. If
Drive converts it to a Google format, check the result: conversion usually keeps
direction (Word's `w:bidi` becomes RTL paragraphs), but verify by reading the
converted file back rather than assuming. Producing those files correctly is
covered in rtl-generated-files.

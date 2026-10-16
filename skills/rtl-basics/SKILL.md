---
name: rtl-basics
description: Core right-to-left rules for any output containing Hebrew, Arabic, Farsi or Urdu — how direction is set, why it silently fails, how to keep numbers, dates, phone numbers, prices and Latin text from being reordered, and the Israeli formatting conventions. Use whenever you write RTL text into anything other than this chat, alongside the surface-specific skill (rtl-email, rtl-google-workspace, rtl-calendar, rtl-generated-files).
---

# RTL basics

These rules apply to every surface. The surface skills add what is specific to
email, Google Docs/Sheets/Slides, calendar events and generated files.

## When this applies

Any time Hebrew, Arabic, Farsi or Urdu text leaves the chat: an email, a document,
a spreadsheet, a calendar event, a file name, a PDF, a chart. If the user writes to
you in Hebrew, assume what you produce for them is Hebrew and RTL unless they ask
otherwise. Mixed documents (an English document quoting Hebrew) need it too, for
the Hebrew parts.

## The failure is silent

Wrong direction never raises an error. The tool call succeeds, the text is all
there, and every character is correct. What is wrong is the order on screen: lines
start at the left, the full stop sits at the wrong end, and `02-123-4567` comes out
as `4567-123-02`. It survives review because it is still readable, and the
recipient notices it immediately. **A successful tool call is not evidence that the
direction is right.** Only setting direction explicitly and reading it back is.

## The rules

1. **Set direction explicitly; never rely on auto-detection.** Every format has a
   direction switch (HTML `dir="rtl"`, a Docs paragraph `direction`, a sheet's
   `rightToLeft`, Word's `w:bidi`). Formats that default to left-to-right do so
   silently. Google Docs and Slides paragraphs default to LTR and **do not inherit
   direction** from their neighbours. Each one needs its own.
2. **Set it per block, not once at the top.** Every paragraph, table cell and list
   item that holds RTL text carries its own direction. Inheritance is unreliable
   across clients and editors and is lost when content is copied or re-flowed.
3. **Isolate multi-part numbers.** A plain number inside RTL text is fine. Anything
   with internal separators gets reordered: phone numbers, account and branch
   numbers, ID numbers with check digits, IBANs, dates, date ranges, times,
   version numbers, case references. Wrap each one as an LTR run (HTML
   `<span dir="ltr">…</span>`), or put it alone in its own cell or line.
4. **Latin text at the start or end of an RTL paragraph** drags the punctuation
   to the wrong side. Latin in the middle of a paragraph is fine. Wrap a leading or
   trailing Latin run in an LTR span, or reword so the paragraph starts and ends in
   RTL script.
5. **Where you cannot set direction** (plain text, titles, subjects, file names,
   SMS), put the RTL text first. If that is not enough, start the line with the
   invisible RIGHT-TO-LEFT MARK, U+200F. Insert the actual character, not `&rlm;`
   or `‏` written out as text. Don't use it in file names (see
   rtl-google-workspace).
6. **Use a font that covers the script.** A font without Hebrew falls back one
   character at a time, so a word can render in two faces, or as empty boxes.
   Hebrew: Arial, or Noto Sans Hebrew / David Libre / IBM Plex Sans Hebrew where
   you control the fonts. Arabic and Farsi: Tahoma, then Arial.
7. **Give RTL text more line height**: about 1.6–1.7 rather than the Latin default.
8. **Signatures, addresses and code stay LTR.** A Latin-script name or email
   address under a Hebrew body goes in its own LTR block. Don't force it RTL to
   match.
9. **Verify by reading back.** After writing, read the result back through the
   same tool (the paragraph style, the sheet property, the email's body) and check
   the direction is what you set. If your tool cannot set or read direction, tell
   the user plainly rather than reporting success.

## Israeli conventions (Hebrew output)

| Item | Convention | Note |
|---|---|---|
| Dates | `16.10.2026` or `16/10/2026` (day first) | Never month-first. In running text the Hebrew month name avoids ambiguity: 16 באוקטובר 2026. Isolate numeric dates as LTR. |
| Times | 24-hour, `18:30` | |
| Currency | `120 ₪` or `120 ש"ח`, symbol after the number | Use `₪`, not `NIS`, in Hebrew text. In English text `ILS 120` or `₪120` is fine. |
| Phone numbers | `02-123-4567`, `050-123-4567`, `*2700` | Always an LTR run. Star numbers (`*1234`) are short codes; keep the asterisk in front. |
| Teudat zehut | 9 digits | Its own cell or an LTR run. |
| Week | Sunday to Thursday is the working week. Friday is a short day. Saturday is Shabbat. | Matters for deadlines and "next business day". |
| Quotation marks | Hebrew abbreviations use the gershayim: ש"ח, ת"ז, בע"מ | Don't "correct" them to English quotes. |

## Register

Ordinary Hebrew is markedly more informal than English: second person singular,
שלום or היי, and תודה as a full sign-off. Don't translate English boilerplate ("I
hope this finds you well"). Writing to an institution (a bank, the health fund, the
municipality, Bituach Leumi, a court) is a different register: open with לכבוד,
add a הנדון: line, close with בכבוד רב, and include the identifiers they file by.
Where it is unclear, formal is the safer mistake.

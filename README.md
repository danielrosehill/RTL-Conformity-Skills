# RTL Conformity Skills

Agent skills that keep Hebrew, Arabic, Farsi and Urdu text right-to-left on every
surface an AI agent writes to. Wrong direction never raises an error: the tool call
succeeds and the text is all there, but it reads left-to-right and phone numbers,
dates and account numbers come out reordered. These skills tell the agent which
switch to set on each surface, and how to check it took.

Instruction-only (no MCP server, no scripts). The same files work in **Claude**
(Claude Code, Cowork, claude.ai) and **ChatGPT** (Business/Enterprise plugin
marketplaces import this Claude-format layout as-is).

| Skill | Covers |
|---|---|
| `rtl-basics` | The core rules: explicit direction per block, isolating multi-part numbers, RLM marks, fonts, read-back. Israeli conventions for dates, currency, phone and ID numbers, and the formal/informal register split. |
| `rtl-email` | HTML bodies with `dir="rtl"` on every block, per-tool notes for Gmail-style tools and Resend, the escaped-HTML failure, plain-text fallback, subjects, read-back after sending. |
| `rtl-google-workspace` | Google Docs paragraph direction (defaults to LTR and is not inherited), RTL sheets and cells in Sheets, Slides paragraphs, Hebrew file and folder names in Drive. |
| `rtl-calendar` | Event titles, descriptions and locations in Hebrew; `Asia/Jerusalem` time zone, day-first dates, Shabbat and holidays. |
| `rtl-generated-files` | Word (`w:bidi`/`w:rtl`), PDF, Excel (`rightToLeft`, `readingOrder`), CSV with a BOM, matplotlib charts, and the reversed-text and font-substitution failures. |

Skills refer to tools by capability ("your email tool"), never by a vendor's tool
name, so they work with whichever Gmail, Drive, Calendar or Resend integration the
agent has.

## Install

**Claude Code:**

```
/plugin marketplace add danielrosehill/RTL-Conformity-Skills
/plugin install rtl-conformity@rtl-conformity
```

**ChatGPT Business / Enterprise:** in *Admin console → Plugins → Marketplaces*, add
this repository as a marketplace source, or vendor `skills/` and `.claude-plugin/`
into an existing marketplace repository.

## Provenance

Grew out of [Claude-RTL-Email](https://github.com/danielrosehill/Claude-RTL-Email),
which sends RTL email with saved signature profiles and local preview scripts, and
[Claude-Hebrew-Correspondence-Plugin](https://github.com/danielrosehill/Claude-Hebrew-Correspondence-Plugin)
(typeset Hebrew letters). This repo carries their portable rules without the local
tooling, and extends them to Docs, Sheets, Slides, Drive, Calendar and generated files.

Verified on 2026-10-16: the Google Docs, Sheets and Slides direction fields (from
Google's API discovery documents), and the openpyxl, python-docx and python-bidi
calls quoted in `rtl-generated-files`. Email behaviour comes from the Claude-RTL-Email
tests (Chromium renders, 2026-08-05; a real send, 2026-09-01). Outlook behaviour and
which HTML Google Calendar keeps in descriptions are established practice, not
tested here. Each skill says which is which.

## License

MIT

# RTL-Conformity-Skills: agent notes

Public Claude plugin `rtl-conformity`: instruction-only skills that make agents
produce correct right-to-left output on every surface (email, Google Docs, Sheets,
Slides and Drive, calendar, generated files). Created 2026-10-16.

## Consumers

- **Claude:** installable directly. The repo is its own marketplace (`.claude-plugin/marketplace.json`, source `./`).
- **The Rosehill household ChatGPT Business workspace:** vendored into
  `danielrosehill/Residence-Skills-Marketplace-ChatGPT` by
  `Residence-Skills/skills/publish-to-chatgpt/scripts/publish.sh`, through its
  `EXTERNAL` map. That vendors the pushed `main` branch, so push here first, then run
  publish.sh from Residence-Skills.

## Invariants

- **Instruction-only.** No `.mcp.json` (ChatGPT would mark the plugin *Desktop only*),
  no scripts the skills depend on. Bulky material goes in a skill's own `references/`.
- **Framework-agnostic.** Agent Skills spec: frontmatter `name` equals the folder
  name, and `description` says what the skill does and when to use it. Name
  capabilities ("your email tool"), never vendor tool names (`mcp__…`, connector
  names).
- **Each skill stands alone.** Surface skills point to `rtl-basics` by name for the
  shared rules, and repeat only the essentials they cannot work without.
- **Say what is verified.** Date each fact (absolute dates), and mark established
  practice as untested.
- **Public repo:** no personal identifiers, addresses or credentials. Examples use
  placeholder numbers.
- Bump `version` in both `.claude-plugin/plugin.json` and `marketplace.json` when
  skill content changes.

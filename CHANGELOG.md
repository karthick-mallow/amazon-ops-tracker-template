# Changelog

## 0.10.0 — 2026-11-17

Kit and template ship this version together. No `tracker.csv` column changes (schema stays 4).

### Added
- **Schema version:** `config.md` gains `Tracker schema: 4`. AI tools read it before every write
  and migrate step by step from it (`AGENTS.md` section 7, with the schema 1–4 history); the
  header is only inspected when the line is missing. A tracker newer than the AI's instructions
  is never written to.
- **Tracker check:** a GitHub Action (`.github/workflows/check-tracker.yml`, standard-library
  Python in `.github/scripts/check_tracker.py`) runs on every push touching `tracker.csv`,
  `config.md` or `activities/`. It checks the header, column count, blank cells, ID / date /
  follow-up formats, allowed Type / Mode / Status / Focus values, and that each Page exists. On
  failure it opens one issue labelled `tracker-check`; the next pass closes it. Rows from before
  User/Mode (`User = unknown`) get warnings, not errors, for old values.
- AI tools look for an open `tracker-check` issue at the start of a conversation and, with the
  seller's OK, fix only what it lists — never figures.
- `KwickMetrics focus` → `Focus` (0.8.0) is recorded as the one legacy rename; columns are
  otherwise only added.

### Migration — existing seller trackers
1. Optional: add **Issues: Read-only** to each tool's GitHub key so the AI sees check failures,
   and **Workflows: Read and write** to the key Claude uses, so it can add the check.
2. Tell Claude "update my AGENTS.md". In one commit it updates the instructions, migrates the
   tracker to schema 4 if needed, adds `Tracker schema: 4` to `config.md`, and adds the two check
   files. Without the Workflows permission it adds everything else and tells you; you can then
   copy `.github/workflows/check-tracker.yml` from the template repo by hand.
3. The first push runs the check. If it opens a `tracker-check` issue, say "fix the tracker check
   issue".
Grok users: re-paste `PROMPT.md`.

## 0.9.0 — 2026-11-17

Kit (amazon-ops-kit, Amazon Ops Copilot plugin) and template (amazon-ops-tracker-template) ship
this version together.

### Added
- **`User` and `Mode` columns** in `tracker.csv`, after `AI model`. User = who asked for or approved
  the work; Mode = `Chat`, `Voice` or `Schedule`. Never blank on new rows.
- **Users** table in `config.md` (first row = owner, used when the AI can't tell who is asking).
- `Mode` line in the activity page header; `Executed By` now holds the same name as `User`.
- Scheduled runs: changes through a connected tool are logged automatically; anything else only
  when the scheduled task's instructions say so (`Logged: By the scheduled task's instructions`).
- Commit messages and `STATE.md`'s "Last updated" line include user and mode.
- load-context, review-followups and impact-dashboard read or show User / Mode.

### Migration — existing seller trackers
Nothing to do by hand. On the next logged activity (or when you tell Claude "update my
AGENTS.md"), the AI will, in one commit:
1. add `User` and `Mode` after `AI model` in `tracker.csv`, filling `unknown` for every older row
   (no guessed values);
2. add a **Users** section to `config.md` above **Goals and preferences**, with the owner's row;
3. (with "update my AGENTS.md") replace `AGENTS.md`, `README.md`, `PROMPT.md` and `TEMPLATE.md`
   with the 0.9.0 copies.
Chat apps using `PROMPT.md` (e.g. Grok): re-paste the updated prompt into their custom
instructions.

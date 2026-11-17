# Changelog

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

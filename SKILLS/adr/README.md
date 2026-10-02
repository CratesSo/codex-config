# adr
- Auto-create Architecture Decision Records for durable repo decisions.
- Keep ADR numbering, links, supersession, and index updates consistent.

## How to use
1. Invoke `$adr` when a durable change affects architecture, tooling, data shape, workflow, repo conventions, or agent behavior.
2. Let the skill create or relate ADRs under `MEMOS/adr/`, then update the ADR index.

### Examples
- `Use $adr for this workflow convention change.`
- `Use $adr to record this tooling decision.`

## Install
`npx skills@latest add https://github.com/CratesSo/codex-config/tree/main/SKILLS/adr`

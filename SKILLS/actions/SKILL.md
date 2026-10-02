---
name: actions
description: "Manage workspace actions in .codex/environments/environment.toml."
---

# actions

## Workflow

1. Resolve target workspace.
2. Read `.codex/environments/environment.toml`.
3. Parse existing `[[actions]]` blocks.
4. Infer user requested operation: create/add, inspect/list/show, update/edit/change, remove/delete
5. Extract or confirm: action `name`, `icon`, `command`
6. Run static/light verification before mutating.
7. Report final action name, icon, target file, and command used.

## Source Of Truth

- Primary file: current workspace `.codex/environments/environment.toml`
- `local-env-recent-actions-by-key` in `~/.codex/.codex-global-state.json` isn't source of truth.

If environment file is missing and user asked for mutating action change, create with:

```toml
# THIS IS AUTO GENERATED. DON'T EDIT MANUALLY
version = 1
name = "<workspace name>"

[setup]
script = ""
```

Then add requested `[[actions]]` block.

## Allowed Icons

- `run`
- `test`
- `tool`
- `debug`

## Action Schema

Canonical action block:

```toml
[[actions]]
name = "Restart Dash"
icon = "run"
command = "cd /path && npm run dev"
```

Use `command = "..."` for short single-line commands.

Use TOML multi-line strings for longer commands:

```toml
[[actions]]
name = "Tests"
icon = "test"
command = '''
npm run lint
npm run test
'''
```

Preserve all existing non-action content. Only add/edit/move/remove the `[[actions]]` blocks required for request.

## Natural-Language

Default to plain-English requests:

- "add Restart Dash action for this workspace"
- "show current Codex actions"
- "change Restart Dash to use debug icon"
- "remove Tests action"

Infer missing low-risk details when obvious.

Ask before proceeding if high-impact ambiguity remains:

- same-name action already exists
- target workspace is unclear

## Conflict Policy

If action with same name already exists:

- don't overwrite silently
- show existing action
- ask whether to update it, rename the new one, or stop

## Verification

Do static/light checks only. Never run action command.

Check for:

- target directories referenced by `cd`
- wrong repo root
- `npm run ...` in path without `package.json`
- `uv run`/`cargo`/`python -m` or app binary commands whose likely cwd is wrong
- broken shell quoting or unmatched quotes

## Operation Rules

### Show/List

- Read target environment file.
- Summarize each action with `name`, `icon`, and command shape.
- If no actions exist, say so plainly.

### Create

- Add one new `[[actions]]` block.
- Keep existing order unless the user asked for placement.
- Prefer appending the new block after existing actions.

### Update

- Edit only named action block.
- Preserve untouched fields/formatting when possible.
- If request changes only one field, don't rewrite whole file unnecessarily.

### Delete

- Remove only named action block.
- Preserve rest of file exactly.

## Response Contract

For read-only requests:

- report target file
- list found actions or say none exist

For mutating requests:

- report exact action name, icon, target file
- mention any assumptions locked during verification

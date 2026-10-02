# Explorer configuration

The published `explorer.toml` intentionally omits local-only skill settings. Add the following blocks after the closing `developer_instructions` triple quote in your live `~/.codex/agents/explorer.toml` file.

## Disable Code and ADR

```toml
[[skills.config]]
path = "/Users/admin/.codex/skills/code/SKILL.md"
enabled = false

[[skills.config]]
path = "/Users/admin/.codex/skills/adr/SKILL.md"
enabled = false
```

## Enable Code and ADR

Use the same blocks with `enabled = true`. If either block already exists, change its value instead of adding a duplicate.

## Install

Copy the published `explorer.toml` into `~/.codex/agents/`, then add the preferred local-only blocks above.

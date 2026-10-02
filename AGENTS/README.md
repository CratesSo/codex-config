# Agents

Public Codex global instructions and custom agent definitions in the codex-config repository.

## Install

| File | Purpose | Install |
| --- | --- | --- |
| AGENTS.md | Global Codex instructions. | curl -fsSL https://raw.githubusercontent.com/CratesSo/codex-config/main/AGENTS/AGENTS.md -o ~/.codex/AGENTS.md |
| explorer.toml | Maps multi-module evidence, canonical authority, callers, and control flow. | curl -fsSL https://raw.githubusercontent.com/CratesSo/codex-config/main/AGENTS/explorer.toml -o ~/.codex/agents/explorer.toml |
| reviewer.toml | Reviews correctness, regressions, and security risks. | curl -fsSL https://raw.githubusercontent.com/CratesSo/codex-config/main/AGENTS/reviewer.toml -o ~/.codex/agents/reviewer.toml |
| worker.toml | Handles bounded workspace changes with clear acceptance criteria. | curl -fsSL https://raw.githubusercontent.com/CratesSo/codex-config/main/AGENTS/worker.toml -o ~/.codex/agents/worker.toml |
| worker_heavy.toml | Handles risky work and complex cross-component changes. | curl -fsSL https://raw.githubusercontent.com/CratesSo/codex-config/main/AGENTS/worker_heavy.toml -o ~/.codex/agents/worker_heavy.toml |

Create the destination directories before installing:

mkdir -p ~/.codex/agents

Each TOML contains the portable definition ending at its developer_instructions section. Local-only configuration is kept in the live ~/.codex/agents source.

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

## Subagent guidance

Add the following to `~/.codex/config.toml`. If `[features.multi_agent_v2]` already exists, add or replace these keys within that table instead of adding a second table. `enabled = true` enables the multi-agent tools.

- `usage_hint_text` accompanies the `agents` tool namespace. It defines delegation rules, task naming, and the initial task-message template.
- `root_agent_usage_hint_text` guides the root agent's choice of subagent roles and when to use a coordinator.

These settings guide orchestration; each agent's own instructions remain in its `~/.codex/agents/*.toml` file.

```toml
[features.multi_agent_v2]
enabled = true

usage_hint_text = """
=== Delegation Rules ===

Stay local for quick/simple searches and edits.

Set `fork_turns: "none"` when spawning agents.

When delegating new assignment, spawn fresh agent. Reuse existing agent only for direct follow-ups to its previous assignment: fixing failures or review findings, finishing incomplete work, etc.

Don't interrupt agents or message with `agents.send_message` before they finish unless must be cancelled or steered.

=== Spawn Template ===

Include subagent identifier inside task name:
 explorer = e
 coordinator = c1
 worker = w
 worker_heavy = h
 reviewer = r

Example: `task_name: "e_authority_map"`

When spawning multiple parallel coordinators, increment the coordinator number in their task name.

Use this structure for initial spawn message:

TASK: describe task with actionable info and stop condition; include relevant canonical path for worker agents if already known

CONTEXT: provide compact most relevant known info that helps show narrow/fast path: current state, files, line ranges, etc.

CONSTRAINT: State implementation boundaries, overlapping work, and anything explicitly out of scope; don't repeat or rephrase info from <global_agents.md> or <local_agents.md>.
"""

root_agent_usage_hint_text = """
=== SUB-AGENT ROLES ===

Default to explorer for cross-module tracing and authority mapping.
Use coordinator only after the parent defines at least 3 independent implementation tasks that benefit from concurrent worker or worker_heavy agents. Include each task's scope, write ownership, dependencies, and acceptance criteria. Otherwise delegate directly to worker or worker_heavy.
Use worker for bounded implementation.
Use worker_heavy for high-risk or architecturally complex implementation.
Use reviewer only for major/risky review outside a completed coordinator scope.
"""
```

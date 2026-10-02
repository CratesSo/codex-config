---
name: handoff
description: "Create a user-configured continuation task from current context."
---

# Instructions
1. Infer current goals, completed work, settled decisions, task-specific constraints, key anchors, and blockers from current thread context.
2. Build a continuation packet using the template below and keep it focused on what the next thread needs to continue cleanly. Omit any reference or mention of Codex Skills, including `$skill` tags, `SKILL.md`, and skill names.
3. Call `list_projects`. Identify the saved project matching the current workspace; do not guess a project id.
4. If any setting is not explicitly provided, use one `request_user_input` round containing only the missing questions below:
 - `Where should the continuation run?`: `Current project, local (Recommended)`, `Current project, worktree`, `Another saved project`.
 - `Which model should execute it?`: `Sol`, `Terra (Recommended)`, `Luna`.
 - `Which reasoning effort should be used?`: `Low`, `Medium (Recommended)`, `High`.
5. If `Another saved project` is selected, use one follow-up `request_user_input` question to select it from the saved-project list. Offer up to three matching project names and require an exact selected name before resolving its `projectId`; do not guess.
6. Create a new, user-owned continuation task with `create_thread`:
 - Set `target.type` to `project` and use the selected `projectId`. If no selected saved project exists, report that blocker instead of creating a projectless task.
 - Set `target.environment.type` to the selected `local` or `worktree` destination. Omit `startingState` unless the user explicitly requests an existing working tree or branch.
 - Map the selected model to `gpt-5.6-sol`, `gpt-5.6-terra`, or `gpt-5.6-luna`, and set the selected reasoning effort as `thinking`.
 - Set `prompt` to the continuation packet plus an instruction to continue from that context.
7. Creation is asynchronous. If a `threadId` is returned, use `wait_threads` once to observe its initial progress without narrating an unchanged snapshot. If a worktree is still being prepared and the result contains a `clientThreadId`, do not pass it to thread-specific tools.
8. After successful dispatch, output only one creation directive:
 - `::created-thread{threadId="<thread id>"}` when a ready thread id is returned.
 - `::created-thread{clientThreadId="<client thread id>"}` when worktree setup is pending.

## Handoff Template
Use this template for new thread:

```text
CONTEXT:
This is context from a previous thread. Await further instructions.

PREVIOUS THREAD SUMMARY:
<detailed summary of important work completed so far>

CURRENT TASK(S):
<current objective(s) to carry forward>

SETTLED DECISIONS:
- <locked decisions, constraints, or conventions>

KEY ANCHORS:
- <important files, symbols, commands, or facts>

CURRENT BLOCKERS:
- <material blockers (omit if none)>
```

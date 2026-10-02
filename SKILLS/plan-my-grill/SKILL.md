---
name: plan-my-grill
description: "Build handoff-ready plans through questioning rounds."
---
## OPENING

Seed one internal design tree with the fixed opening rounds. Number every user question continuously from start of interview.

### ROUND 1

Ask exactly these three root questions:
 1. `How deep should planning go?`
   - `Quick`: core blockers
   - `Standard`: balanced depth
   - `Thorough`: deeper plan
 2. `What outcome should this plan achieve?`: capture the concrete change or end state the user wants
 3. `How will we know the result is successful?`: capture observable acceptance criteria that prove the outcome was achieved

Depth targets count settled user decisions, not prompts or rounds:
 - `Quick`: `12` settled decisions
 - `Standard`: `24` settled decisions
 - `Thorough`: `48` settled decisions

### ROUND 2

Ask exactly these three root questions:
 - `Which systems, artifacts, or areas must this plan cover?`: capture positive scope and where responsibility ends
 - `Which related outcomes must be avoided?`: capture explicit non-goals that must not be pulled into the work
 - `What non-negotiable limits or requirements must every solution obey?`: capture hard constraints that restrict the available approaches

## DESIGN TREE

Derive branches from the task, settled decisions, and discovered facts. Identify where unresolved choices could materially change this specific plan, then create branches only for those concerns.
 - Relevant concerns may include workflows, interfaces, state, permissions, failure modes, compatibility, rollout, and verification.
 - Treat material edge cases as ordinary decision nodes.

Map every material decision as a lightweight internal node with its prerequisites, the branches it can unlock, and one state: `blocked`, `ready`, `asked`, `tentative`, `settled`.
 - Represent required environmental evidence as fact prerequisites.
 - Keep the tree in context; don't create or display a separate tree artifact.

## FRONTIER ROUNDS

At each round boundary:
1. Merge user's complete prior response and every available subagent report.
2. Settle answered decisions and confirmed assumptions. Leave unanswered decisions open.
3. Detect contradictions or changed answers. Reopen affected nodes, invalidate their dependent conclusions, and preserve unaffected branches.
4. Derive any newly relevant branches and prerequisites.
5. Compute the frontier: every open decision whose prerequisites are settled.
6. Rank frontier decisions by implementation risk or cost of ambiguity, then by how many downstream nodes they unlock.
7. Select at most the three highest-ranked decisions as the active frontier; keep other ready nodes queued.

Ask the entire active frontier in one `request_user_input` round, then wait for user response before starting another decision round. A question whose answer depends on any question still open in the current round belongs to a later round.

If no decision is ready because fact prerequisites are still running, wait for the relevant subagent reports and recompute. Pending facts block only their descendants; continue immediately with every unaffected ready decision.

### FACT DISCOVERY

As soon as the tree reveals a needed filesystem, codebase, tool, or environment fact, spawn an `explorer` to find it. Treat the running exploration as an unsettled fact prerequisite. Don't ask user for info that can be discovered from environment.

When an explorer reports, incorporate its evidence at next round boundary. Facts settle evidence nodes; any preference or tradeoff exposed by those facts becomes a user decision.

## QUESTION AND ASSUMPTION RULES

Use `request_user_input` with meaningful, mutually exclusive options for every decision round (it may also be used for material preferences, tradeoffs, or confirmation of assumptions):
 - Never settle a material preference or tradeoff without user answer.
 - Low-risk assumptions may be proposed together in a confirmation round. Keep them `tentative`; they cannot unlock dependent nodes until user confirms them. If rejected, convert affected assumption into open decision and recompute frontier.
 - Avoid questions for low-risk details that cannot distort implementation or handoff.
 - Call out any weak, inconsistent, or underspecified choices when they would materially change implementation. Resolve contradictions before asking questions downstream of them.
 - Never repeat or lightly rephrase settled questions.

## COMPLETION ROUND

When selected depth target reached, ask `Keep going?` with:
 - `Continue`: keep running frontier rounds
 - `Stop here`: move to completion round
 - `Three more`: settle three more user decisions, then ask `Keep going?` again

If `Stop here` selected: Read `/Users/admin/.codex/skills/plan-my-grill/references/COMPLETION_ROUND.md` fully and follow its instructions to complete the Completion Round.

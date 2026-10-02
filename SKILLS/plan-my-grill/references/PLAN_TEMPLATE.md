# How to Write the Plan

Use the `Example Template` and guidance below to create a handoff-ready Markdown plan at `plans/<descriptive-slug>.md` in current workspace. Replace every instructional placeholder with concrete details from the planning interview.

The plan must be detailed enough for an agent to implement without reopening settled decisions or guessing at requirements.

## How to Format the Steps

For `## Implementation steps`, organize the work as chronological steps ordered by dependency. Number steps from `1` as `### STEP N - <title>`. Put `STATUS:` followed by backticked `NOT STARTED` below each title, then describe the action, affected artifacts, required behavior, and completion condition. Preserve sequential numbering and dependency order when adding or removing steps.

Under each step description, include applicable `Non-Goals` and `Hard Constraints`. Always include `Reuse and Integration` on non-verification steps.
- `Non-Goals`: list related outcomes that must not be pulled into the step.
- `Hard Constraints`: record every non-negotiable limit or requirement that a step must obey.
- `Reuse and Integration`: identify existing files, symbols, abstractions, workflows, or canonical paths to reuse or extend. State how the step integrates with them and which duplicate, redundant, or parallel paths must not be created. If nothing reusable is known, require inspection before adding a new path.

## The Verification Step

Make `Verification` the final step: describe the checks that demonstrate implementation satisfies constraints and planned outcome. Keep verification proportional to changes and make expected results explicit.

## Gather Evidence After Drafting

1. **Write the draft.** Create the initial artifact before spawning `navigator` agents. Include `- Reuse and Integration: AWAITING NAVIGATOR` under every non-verification implementation step and mark the verification checklist `AWAITING NAVIGATOR`.

2. **Create reuse step sets.** Target every non-verification step during the initial pass; later passes target only steps identified by **Stabilize implementation**. Start each set with the lowest-numbered unassigned target. Add up to two lowest-numbered unassigned targets that directly depend on it or involve the same affected artifacts, workflow, or integration boundary. If that doesn't fill the set, add the lowest-numbered unassigned targets until it contains three. Assign every target once; the final set may contain fewer than three.

3. **Start investigations.** Give one navigator the artifact path and instruct it to locate existing tests, validation commands, fixtures, integration boundaries, and observable behaviors for the verification step. Immediately fill every other free subagent slot with reuse navigators from the step-set queue. Give each reuse navigator the artifact path and exact step numbers and titles it owns; it maps the existing code, canonical paths, abstractions, workflows, and integration points needed to complete those steps' `Reuse and Integration` fields.

4. **Run the rolling queue.**
   - Immediately fill every free subagent slot from the queued step sets.
   - Never wait while both a free subagent slot and queued step set exist.
   - When a navigator finishes, record its report and immediately refill its slot from the queue. Treat the verification navigator's slot the same way after it finishes.
   - Continue until the queue is empty and every active navigator has reported.
   - Keep the draft's step structure stable while investigations run.

5. **Merge reuse evidence.** Replace each `Reuse and Integration` marker with step-keyed evidence from its navigator. When nothing applies, record what was inspected and why no existing implementation applies; justify a new path only when the step creates one.

6. **Stabilize implementation.** Whenever evidence creates or materially changes an implementation step, identify that step and every other step whose dependency or integration evidence may be affected. Mark each identified `Reuse and Integration` field `AWAITING NAVIGATOR`, rebuild those steps into sets using **Create reuse step sets**, run the rolling queue, and merge those reports. Repeat until a reuse merge no longer creates or materially changes steps.

7. **Finalize verification.** Give a navigator the artifact path and initial verification report, then instruct it to review verification against the enriched artifact. Wait for and record its report, then replace the `Verification` marker with its completed checklist or update the existing checklist during a repeated review.

8. **Resolve verification changes.** If the final verification report requires creating or materially changing an implementation step, apply those changes to the artifact, then use **Stabilize implementation** for each changed step and every other step whose dependency or integration evidence may be affected. After implementation stabilizes, perform another final verification review.

9. **Finalize the artifact.** Ensure every navigator report is merged and no `AWAITING NAVIGATOR` marker or instructional placeholder remains. Confirm every implementation step has a title, exact status field, complete description, and completed `Reuse and Integration` field; exempt `Verification` from `Reuse and Integration`, require its completed checklist, and keep it as the final step. Confirm numbering is sequential and dependency order is preserved. Remove optional fields that weren't filled. Navigator findings are technical evidence only; don't let them settle user preferences, scope, or tradeoffs.

## Example Template

```markdown
# <Descriptive Plan Title>

<summary of plan so the agent understands overall intent and desired behavior/outcome>

## Implementation steps

### STEP 1 - <title>
STATUS: `NOT STARTED`

<describe step, including relevant confirmed assumptions from interview>

- Non-Goals: <describe non-goals>
- Hard Constraints: <describe hard-constraints>
- Reuse and Integration: `AWAITING NAVIGATOR`

### STEP 2 - <title>
STATUS: `NOT STARTED`

<describe step, including relevant confirmed assumptions from interview>

- Non-Goals: <describe non-goals>
- Reuse and Integration: `AWAITING NAVIGATOR`

### STEP 3 - Verification
STATUS: `NOT STARTED`

`AWAITING NAVIGATOR`
```

# Goal

Implement the plan defined in `<PLAN_ARTIFACT_PATH>`.

Treat the plan as the implementation, completion, and validation specification.

Complete each step in order until every step is marked `DONE` in the plan artifact.

Proceed without waiting for user input. Use best judgment and continue when a decision is needed. Stop only when credentials are required or materially different choices require user direction.

## Status Tracking

- Update status directly in the plan artifact.
- Set a step's status line to exactly ``STATUS: `IN PROGRESS``` before starting it.
- Set the status line to exactly ``STATUS: `DONE``` only after implementation, reviewer findings, and validation are complete.
- Keep only one implementation step `IN PROGRESS` at a time.

## Avoid Any Process or Safety Theater

- Treat step items as behavioral requirements, not quotas for code or tests.
- Reuse canonical components and avoid duplicate, redundant, or parallel implementations.
- Don't add speculative hardening or unreachable-state handling.

## Validation and Review

- Don't add validation already owned by unchanged canonical components.
- Expand validation only because of a concrete failure or changed shared boundary.
- If creating a test, limit it to the minimum work necessary to prove behavior.
- After each implementation step, spawn a `reviewer` to audit it for completeness, correctness, and regressions.
- Remediate every actionable finding.
- If remediation materially changes the step, repeat reviewer audit and remediation until no actionable findings remain.
- Run the step's required validation before marking it `DONE`.
- Commit the completed worktree changes before starting the next step.

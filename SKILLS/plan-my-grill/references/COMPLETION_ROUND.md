# Completion Round

Ask `Planning complete: How do you wish to proceed?`, offering without marking any as (recommended) in exact order:
 `Implement without writing plan`
 `Write plan only`
 `Write plan and implement`

If `Implement without writing plan` selected:
 Implement immediately in current task using chat context.

If `Write plan only` selected:
 Read `/Users/admin/.codex/skills/plan-my-grill/references/PLAN_TEMPLATE.md` fully and follow it to create plan artifact.

If `Write plan and implement` selected:
  Follow `Write plan only` workflow. After artifact is finalized according to `/Users/admin/.codex/skills/plan-my-grill/references/PLAN_TEMPLATE.md`, use one `request_user_input` round with exactly this question without marking any option as (recommended):
   `Where should the plan be implemented?`: `In this chat`, `New worktree task`, `New local task`

If `In this chat` selected:
 Read only lines 11–16 (`Status Tracking`) of `/Users/admin/.codex/skills/plan-my-grill/references/GOAL_TEMPLATE.md` and follow them, then implement immediately in current task using plan artifact.

If `New worktree task` or `New local task` selected: Use an additional `request_user_input` round with exactly these three questions:
 `Which model should execute the plan?`: `6-Luna`, `6.1-Sol`, `6-Astra`
 `Which reasoning effort should be used?`: `Low`, `Medium`, `High`
 `How should implementation run?`: `Goal mode`, `Standard execution`

If `Goal mode` selected:
 Create selected task type with chosen model and reasoning effort. Its concise, self-contained bootstrap prompt must include plan artifact's absolute path and instruct new task to:
   Read `/Users/admin/.codex/skills/plan-my-grill/references/GOAL_TEMPLATE.md` fully without modifying it.
   Replace `<PLAN_ARTIFACT_PATH>` in memory with provided artifact path.
   Start goal mode using complete rendered prompt before implementing.

Root agent must not read or render `/Users/admin/.codex/skills/plan-my-grill/references/GOAL_TEMPLATE.md`.

If `Standard execution` selected:
 Read `/Users/admin/.codex/skills/plan-my-grill/references/GOAL_TEMPLATE.md` fully without modifying it. Replace `<PLAN_ARTIFACT_PATH>` in memory with plan artifact's absolute path, then create selected task type with chosen model and reasoning effort using complete rendered prompt as its initial prompt. Don't instruct new task to start goal mode.

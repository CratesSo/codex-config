# handoff <img src="./assets/icon.png" width="40" alt="handoff icon">

**Creates a configured continuation task from the active thread context.**

## Note

- This skill uses Codex's `request_user_input` questions to choose the destination, model, and reasoning effort.
- Enable that capability in `~/.codex/config.toml` with `default_mode_request_user_input = true`.

## How to use

1. Ensure `default_mode_request_user_input = true` is set in `~/.codex/config.toml`.
2. Type `$handoff`.
3. Select the destination, model, and reasoning effort when prompted.
4. Continue in the created task.

## Install

`npx skills@latest add https://github.com/CratesSo/codex-config/tree/main/SKILLS/handoff`

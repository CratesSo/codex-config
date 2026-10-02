# code
- Apply strict coding constraints for complex implementation, refactoring, risky edits, and dead code cleanup.
- Keep changes narrow by identifying the canonical path, deleting obsolete code, and avoiding speculative abstractions.

## How to use
1. Invoke this skill for substantial code changes where scope control and cleanup discipline matter.
2. Follow the rules in `SKILL.md` while editing, validating, and removing replaced paths.

### Examples
- `Use the code skill to refactor this module without adding compatibility shims.`
- `Use the code skill to implement the fix and remove the obsolete path.`

## Install
`npx skills@latest add https://github.com/CratesSo/codex-config/tree/main/SKILLS/code`

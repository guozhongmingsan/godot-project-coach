# Project Progress File

## Canonical path

Maintain exactly one operational progress file at:

```text
docs/project-coach-progress.md
```

This file is not a GDD and not a conversation transcript. Keep it concise and overwrite stale current-state fields.

## Canonical schema

```markdown
# Project Coach Progress

## Project
- Goal:
- Godot version:
- Technical stack:

## Current Unit
- Outcome:
- State:
- Blocker:

## Completed
- Unit:
- Evidence:

## Understanding
- Mastered:
- Unresolved:

## Next
- Unit:
```

Use a short list under Completed when multiple units exist. Keep evidence to the command, observed behavior, screenshot description, or explicit user confirmation that established acceptance.

## Read and reconcile

On Start, create the file only after the project goal, Godot version, technical stack, and first unit are known.

On Continue:

1. Read the entire file.
2. Inspect available project files and recent test or runtime evidence.
3. Compare them with Current Unit, Blocker, and Understanding.
4. When they conflict, name the discrepancy and resolve it with project evidence or one focused user question.
5. Resume the earliest incomplete gate. Do not restart mastered work or jump to Next while a blocker or unresolved concept remains.

## Update rules

Update only after one of these meaningful changes:

- implementation evidence is obtained;
- a Grill Me concept becomes Pass, Partial, or Fail;
- a blocker is discovered or cleared;
- the user approves a scope change;
- the current unit completes.

Never mark a unit complete from code inspection alone. Never erase unresolved concepts to simplify the file. When a unit completes, move it to Completed with its evidence, clear the current blocker, and set Current Unit to the single approved next unit.

## Minimal example

```markdown
## Current Unit
- Outcome: Player cannot walk through tree collision tiles.
- State: Repair
- Blocker: Trees are visible but do not block Player.

## Understanding
- Mastered: Input.get_vector movement vector.
- Unresolved: TileSet physics polygon versus TileMapLayer collision_enabled.

## Next
- Unit: Camera2D follows Player.
```

Continue at Repair. Do not reteach movement or start Camera2D.

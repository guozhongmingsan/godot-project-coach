# Godot Project Rules

## File responsibilities

Use these defaults unless the user explicitly approves a project-specific exception:

| Artifact | Owns | Does not own |
| --- | --- | --- |
| `.tscn` | Node composition, essential resource links, initial structure | Large control flow or domain rules |
| `.gd` | Godot lifecycle, input, presentation orchestration, signals, engine glue | Duplicate portable C++ domain rules |
| `.tres` | Configuration, content, tunable data | Control flow or mutable behavior |

“`.gd` thick” means thick relative to `.tscn` on the Godot side. When a portable C++ core exists, keep engine-independent domain rules in C++; let GDScript coordinate Godot Nodes and translate application events.

Prefer programmatic signal connections, resource loading, and animation flow when they materially improve traceability in a complex unit. Do not move simple, stable editor configuration into code merely to avoid the Inspector.

Name every required AutoLoad in L6, including its responsibility and consumers. Never silently add global mutable state.

## L5-L7 truth model

- **L5 — Work Breakdown & Execution Truth:** Define the current small, independently executable Work Unit. Record its stable ID, purpose, source L1-L4 artifacts, dependencies, in-scope work, out-of-scope work, Definition of Done, expected artifacts, priority, and status. L5 is the project's Work Progress Truth.
- **L6 — Human-readable Representation of Implementation Truth:** After L7 is implemented or modified, describe what the implementation actually does using meaningful semantic blocks. Include actual behavior, control flow, state changes, Node and module interactions, important dependencies, Godot framework usage, side effects, failure paths, implementation decisions, potential risks, and any decisions made by L7 that were not defined in L1-L4.
- **L7 — Implementation Truth:** Provide the executable `.gd`, C++, `.tscn`, `.tres`, tests, project settings, and build configuration that determine actual runtime behavior.

For a code-bearing unit, use this primary order:

```text
L5 Work Unit → L7 implementation → verification → L6 Human Review Model
```

L5 is not behavioral pseudocode. Product behavior and acceptance boundaries belong primarily to L2; module responsibilities belong to L3; interfaces, data, state, and error contracts belong to L4.

Formal L6 must be generated or updated from the actual final L7. Any implementation planning written before coding belongs to L4 detailed design or L5 execution notes, not L6.

Never create L6 by translating individual code keywords into prose. Group the implementation into meaningful behavior, state, interaction, error-handling, and framework-integration blocks that help a human review what the code actually does.

## Unit deliverables

For a code-bearing unit, present only the layers the unit needs:

1. The current L5 Work Unit, including scope, exclusions, dependencies, and Definition of Done.
2. The minimal Scene tree and file responsibility changes.
3. L7 implementation and exact configuration.
4. Runtime or test acceptance evidence.
5. L6 that represents the verified final implementation and identifies important risks or undefined decisions.

Do not invent a `.tres`, AutoLoad, abstraction, signal bus, component, or service before the current behavior requires it.

## Node guidance

When teaching a Node, connect it to the active project unit. Include:

- its responsibility in the Scene tree;
- required Inspector fields;
- the smallest relevant GDScript;
- a real game example;
- the boundary versus similar Nodes.

Do not turn a single unit into a catalogue of unrelated Nodes. If the user explicitly requests a node course, teach the requested batch while keeping one shared runnable scene and one acceptance result.

## Common boundary mistakes

| Mistake | Correction |
| --- | --- |
| Put gameplay flow in `.tscn` callbacks and editor wiring | Keep the scene structural; make flow explicit in `.gd` |
| Put mutable runtime state in shared `.tres` definitions | Keep definitions as data; store runtime state in the owning system |
| Make every system an AutoLoad | Use an AutoLoad only for a justified project-wide lifetime |
| Duplicate a C++ rule in GDScript for convenience | Keep one domain source of truth and expose it through the adapter |
| Write L6 as line-by-line translated code | Describe ownership, state transitions, framework calls, and failure paths |

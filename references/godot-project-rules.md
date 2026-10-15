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

- **L5 — Behavioral Truth:** State plain-language rules, exceptions, and boundaries without Godot-specific implementation details.
- **L6 — Human-readable Representation of Implementation Truth:** Describe the selected language and framework structure in readable natural-language pseudocode, aligned closely enough with L7 to expose Nodes, files, ownership, calls, signals, state, errors, and control flow.
- **L7 — Implementation Truth:** Provide the actual `.gd`, C++, `.tscn`, `.tres`, project settings, and build configuration.

L6 may be produced before L7 or reconstructed after a first L7 pass. Judge it by fidelity, not order. Never create L6 by replacing code keywords with prose; a useful L6 explains why control and data cross each boundary.

## Unit deliverables

For a code-bearing unit, present only the layers the unit needs:

1. L5 rules and edge cases.
2. The minimal Scene tree and file responsibility changes.
3. L7 implementation and exact configuration.
4. L6 that faithfully represents the final implementation structure.
5. Runtime or test acceptance evidence.

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

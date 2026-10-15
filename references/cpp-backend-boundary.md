# C++ Backend Boundary

## Meaning of backend

For a single-player Godot game, treat “backend” as non-presentation game rules and simulation. It does not imply a remote server.

Use this dependency direction:

```text
Godot presentation → GDScript application/glue → thin GDExtension adapter → pure C++ core
```

Dependencies point inward. The pure core never imports Godot concepts.

## Adoption gate

Introduce C++ only when at least one observable need exists:

- the user explicitly requires reusable rules across games or engines;
- a domain has stabilized enough to isolate and test;
- profiling identifies a real GDScript bottleneck;
- a required library has a native C++ interface.

An explicit portability goal justifies designing a boundary early. It does not justify migrating unrelated or unstable behavior.

C++ raises the effort required for casual inspection but remains machine code that can be analyzed. Never present it as a reliable anti-reversing, anti-cheat, or trust boundary. Put truly authoritative checks on a controlled server when the product requires them.

Before version-sensitive build or integration guidance, discover the project's exact Godot, GDExtension, godot-cpp, compiler, and build-system versions. Verify compatibility against the matching official documentation rather than assuming current APIs or flags.

## Responsibilities

| Layer | Owns | Must not own |
| --- | --- | --- |
| Godot presentation | Scenes, UI, animation, audio, input, camera, visual feedback | Portable domain rules |
| GDScript application/glue | Node lifecycle, use-case orchestration, signals, Godot-facing state, calls into the adapter | A second implementation of C++ rules |
| GDExtension adapter | Type conversion, ownership transfer, error conversion, registration, calls | Growing domain decisions |
| Pure C++ core | Engine-independent state, rules, deterministic simulation, domain errors | Nodes, scene paths, signals, Godot containers or math types at its boundary |

Battle, family, bloodline, cultivation, economy, production, and card resolution are candidates only when their inputs and outputs can be expressed without the SceneTree.

## Boundary contract

Before implementation, specify:

- input value types and ownership;
- output value types;
- domain error cases;
- determinism and ordering requirements;
- serialization boundary;
- which layer is the single source of truth.

Prefer plain C++ value objects at the core boundary. Convert `Vector2`, `StringName`, `Array`, `Dictionary`, Resources, and Nodes in the adapter instead of leaking them inward.

## First migration sequence

Migrate one small stable rule:

```text
Choose one rule
→ define input, output, and errors
→ write a failing pure C++ test
→ implement the pure C++ rule
→ add a thin GDExtension adapter
→ call it from GDScript
→ verify inside Godot
→ Grill Me on the boundary
```

Do not begin by translating an entire existing GDScript backend.

## Validation layers

| Layer | Evidence |
| --- | --- |
| Pure C++ core | Unit tests for rules, edges, determinism, and errors |
| GDExtension adapter | Boundary tests for conversion, ownership, and error mapping |
| GDScript integration | Tests or headless checks for orchestration and Godot-facing state |
| Scene/UI/animation | Runtime and visual acceptance in the actual game |

A passing core test does not prove the adapter or presentation. A working scene does not prove portable domain correctness. Require the layers touched by the unit.

## Common mistakes

| Mistake | Correction |
| --- | --- |
| Move everything to C++ before rules stabilize | Isolate one stable domain rule first |
| Pass Nodes and scene paths into the core | Pass plain values through the adapter |
| Put business rules in the adapter | Keep it mechanical and thin |
| Keep duplicate GDScript and C++ implementations | Choose one source of truth |
| Claim C++ makes a single-player game secure | Describe only the increased reverse-engineering cost |

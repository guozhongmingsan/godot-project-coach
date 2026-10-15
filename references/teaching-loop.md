# Teaching Loop

## State machine

Follow this order without skipping gates:

```text
Recover → Unit contract → Explain → Implement → Verify → Grill Me
→ Remediate → Reassess unresolved concepts → Record → Unlock
```

| Gate | Required output | Exit condition |
| --- | --- | --- |
| Recover | Current project state and earliest incomplete gate | Progress reconciled with available evidence |
| Unit contract | Exactly four labeled fields: In scope, Out of scope, Observable acceptance, Critical concepts | All four fields appear before implementation and are concrete enough to test |
| Explain | Only concepts needed for the current unit | User can begin the immediate action |
| Implement | Small ordered steps with exact files, Nodes, Inspector fields, or code | Acceptance check can run |
| Verify | Actual versus expected result and evidence | Implementation succeeds |
| Grill Me | Questions testing critical concepts | Every concept is classified |
| Remediate | Targeted correction and one small task | Failed concept is ready for reassessment |
| Record | Concise progress update | Verified result and understanding are preserved |
| Unlock | Exactly one next unit | Current unit is complete |

If a build, test, scene, or runtime behavior fails, enter Repair immediately:

```text
Stop new instruction → collect exact error and reproduction
→ identify root cause → make the smallest scoped correction
→ rerun acceptance → resume the earliest incomplete gate
```

Recording a blocker while continuing architecture or a new lesson is still advancement and is forbidden.

## Unit sizing

Choose a vertical unit that produces one visible, runnable, or testable result. Split broad systems before teaching. For example:

```text
Inventory
→ item data and one in-memory slot
→ pickup into slots
→ stacking
→ drag/drop UI
→ equipment
→ persistence
→ shop transactions
```

Show the sequence when useful, then implement only the first incomplete unit. Do not prebuild future Nodes or interfaces unless the current unit would otherwise enter a known dead end.

Render this contract before implementation:

```text
In scope: behaviors implemented in this unit
Out of scope: requested or tempting behaviors deferred
Observable acceptance: one runnable or testable result
Critical concepts: boundaries the user must later transfer or explain
```

## Grill Me contract

Ask questions that require explanation, prediction, debugging, modification, or transfer. Do not treat term recall or copied code as understanding.

- **Pass:** Explain the correct mechanism and transfer it to a new situation.
- **Partial:** State the right result but miss the mechanism, boundary, or application.
- **Fail:** Show a misconception, guess, or memorize code without understanding it.

Completion requires verified implementation and Pass on every critical concept. Never average away a foundational Partial or Fail.

For each unresolved concept:

1. Name the exact misconception.
2. Re-explain it with a different mental model or example.
3. Give one small prediction or modification task.
4. Reassess only that concept.

Do not repeat questions for concepts already passed.

### Example

If the user says “`CharacterBody2D` owns the visible artwork,” classify it as Fail. Explain that the root owns controlled movement and collision behavior while `AnimatedSprite2D` owns presentation. Ask the user to predict what still works if the sprite is hidden. Do not start TileMap until this boundary passes.

## Turn shape

Keep each coaching response easy to act on:

```text
Current gate
Current unit outcome
Unit contract with all four labeled fields
Immediate action
Acceptance check
```

Add explanation and code only to the degree required by the immediate action. End at the natural gate: wait for implementation evidence, ask Grill Me questions, or remediate one unresolved concept.

## Common mistakes

| Mistake | Correction |
| --- | --- |
| Provide all requested code because the user is rushed | Preserve urgency by shrinking the unit, not removing gates |
| Correct an answer, then start the next lesson | Remediate and reassess before Unlock |
| Ask new questions about mastered material | Reassess only unresolved concepts |
| Mark code as complete because it looks correct | Request runtime or test evidence |
| Continue teaching while a crash remains | Enter Repair immediately |

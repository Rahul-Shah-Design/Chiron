# course-plan (ignore).md schema

Read before writing the plan. The plan is the only state shared between chat and artifacts; if it isn't in this file it doesn't exist at the next boundary.

It is a file named `course-plan (ignore).md` in the build directory (SKILL.md §0), created before module 1 and updated in place at every boundary — by the tutor only; the background builder reads it and never writes it. It is mirrored to the learner's connected folder when there is one.

```markdown
---
topic: <what they asked to learn>
goal: <what they want to be able to do with this — theirs, or the use you picked and named>
domain: <what they are using it on — their project, their data, their students>
hook_attempt: <their first answer to the throughline, quoted>
started: <date>
status: planning | in-progress | closed
---

## Throughline
<the hook scenario's question, verbatim — the performance that shows the goal is met>

Claims — the full answer, written once, here. Modules below reference these by id
and track state; they don't repeat the text.
- C1 <one sentence> — type: fact | rule | principle — module: M1
- C2 ... — module: M1
- C3 ... — module: M2

## Modules

### M1 — <title as a claim, not a topic label>
kind: module | chat-tell          # size gate: see SKILL.md §3
impasse_question: <the one question asked before building>
impasse: <filled after the elicit turn — the wall they hit, or "none, skipped to practice">
claim_states: {C1: untouched | elicited | told | demonstrated, C2: ...}
canonical_floor: [<term>, <list>, ...]   # card fronts at the boundary are this list, verbatim
instrument: <control → variable → visible change, or none>
gate_items: <count, and which claim each sits after>
end_set: <count; from M2 on, which earlier claims it pulls forward>
boundary_question: <printed in full>
delivered: no | yes
log: <one line per boundary — what the answer showed, what moved>

### M2 ...

## Wrong models
- <planned at §2: how people arrive believing this works> → distractor in <module/item>
- <date> <surfaced live, in their words> → <status: confronted | resolved | open>

## Next
<one thing to recommend at close, with rationale>

## Log
- <date> <what changed in the plan and why>
```

Rules:

- Every claim has a `type`, written once under Throughline. Type picks the opening move (SKILL.md §3). Nothing else about the plan is optional.
- `claim_states` moves forward only on evidence: `elicited` when they attempted it, `told` when a module or chat-tell delivered it, `demonstrated` when they used it in a check or boundary answer. A boundary answer can move a `told` claim back to `elicited` — say so in that module's `log`.
- `impasse` is written in the same turn the module is built, before the build.
- Card fronts are read straight from `canonical_floor` at the boundary; there is no separate field to keep in sync. Empty floor, no card block.
- Mark `delivered: yes` in the turn the artifact ships, not later.
- The course is closed when every claim is `told` or `demonstrated` and every module is `delivered`. Not before.
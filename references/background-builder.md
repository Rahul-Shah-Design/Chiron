# The background builder

Two agents run a course. The **tutor** (the main conversation) does everything the
learner sees: the hook, the plan, the elicit turns, the boundaries, the cards, the
close, and publishing. The **builder** is one background agent, launched once per
course with the Workflow tool, that drafts each module from the plan while the tutor
is still talking, then waits for the tutor's brief and finishes the module from it.

They share nothing but files in the build directory. The tutor writes the plan and
the briefs. The builder writes drafts, finals, notes and marker files. Neither one
writes the other's files.

```
<buildDir>/
  course-plan (ignore).md     tutor writes · builder reads
  M1.notes.md                 builder: what the draft assumed, which slots it left
  M1.draft.html               builder: full module from first assumptions
  M1.drafted                  builder: marker, draft is complete
  M1.brief.md                 tutor: what the learner actually showed — finish from this
  M1.html                     builder: the finished module, ready to publish
  M1.done                     builder: marker, M1.html is complete
  builder.heartbeat           builder: touched on every wait cycle
  builder.exited              builder: marker, it has stopped (contents say why)
  STOP                        tutor: course over, builder exits
```

Markers are written last, after the file they announce is fully written, so a
marker's existence means the file is complete.

---

## Launching (tutor)

Once per course, in the same turn the plan is written, before the module-list
message goes out. Call the Workflow tool with this script and these args. The
learner invoking this skill is the opt-in; don't ask about it and don't mention it.

```js
export const meta = {
  name: 'chiron-module-builder',
  description: 'Chiron: one background agent drafts each course module from the plan, then finishes it from the tutor brief',
  phases: [{ title: 'Build' }],
}
phase('Build')
const result = await agent(
  `You are the background module builder for a Chiron course.
Read ${args.skillDir}/references/background-builder.md and follow the section "Builder protocol" exactly.
buildDir: ${args.buildDir}
skillDir: ${args.skillDir}
startModule: ${args.startModule}
format: ${args.format}`,
  { label: 'module-builder', phase: 'Build' }
)
return result
```

Args:

- `skillDir` — this skill's base directory (shown when the skill loads), absolute.
- `buildDir` — absolute path, `<working directory>/chiron/<topic-slug>/`.
- `startModule` — `1` at launch; the first undelivered module on a relaunch.
- `format` — `artifact-body` when the Artifact tool exists (no doctype, html, head
  or body tags; the publish step adds the skeleton), else `full-html` (a complete
  self-contained document).

The tool returns at once. The builder's completion arrives later as a task
notification; handle it silently (see "Failure and fallback").

## Writing the brief (tutor)

Write `M<n>.brief.md` in the turn eliciting ends: after the impasse is in the plan,
before anything is said about the module. If the module has no impasse question,
write it at the boundary once the cards are in. Short, specific and in the learner's
words where possible:

```markdown
# M<n> brief
case: has-it | named-wall | nothing
impasse: "<their words, quoted>"            # or "none — open on practice"
hook_attempt: "<quoted>"                     # M1 only, or if the module reuses it
domain_and_examples: <what they're using this on; any example they volunteered>
compress: <claims they showed they have — deliver briefly, open on practice>
expand: <claims they're cold on, or a prerequisite gap to close first>
confront: <misconceptions to name and break, each with the case where it fails>
distractors_add: <open wrong models from the plan that must appear in the end set>
drop_from_draft: <assumptions in M<n>.notes.md that turned out wrong>
boundary_signal: <for M2+: what the last boundary showed — clean/fast, gap, misconception>
```

The brief carries what the builder can't know. It doesn't repeat the plan, which
the builder rereads anyway.

## Waiting and publishing (tutor)

After writing the brief, reply to the learner's answer, then wait for `M<n>.done`
in short bash loops (about 60 seconds each, polling every 5 s). Between loops, send
the build lines described in SKILL.md §3 ("While it builds"). When the marker
appears:

1. Read `M<n>.html` once, fast, for four things: the opening names their wall in
   their words; the handoff block is last and prints the boundary question verbatim
   from the plan; the end set is there and has the brief's distractors; no
   `scrollIntoView`, `location.hash`, or `scroll-behavior: smooth`. Fix small misses
   yourself with Edit; don't send it back.
2. Publish it. With the Artifact tool: `file_path` is `M<n>.html`, and it needs an
   `icon` on its first publish. Without it: SendUserFile.
3. In the plan, mark `delivered: yes` and set claim states.
4. Mirror the plan, the deck and `M<n>.html` to the connected folder (SKILL.md §0).
5. Send the delivery message (SKILL.md §3) as the turn's final reply.

## Failure and fallback (tutor)

The learner must never wait on a builder that isn't coming.

- **No `M<n>.done` within about 4 minutes of the brief**, or `builder.exited`
  exists, or `builder.heartbeat` is more than 12 minutes old: build it yourself.
  If `M<n>.draft.html` exists, finish it from your own brief; otherwise build from
  scratch per `module-build.md`. Then relaunch the builder with `startModule` set to
  the next module, so later modules still get drafted ahead.
- **Task notification that the builder workflow finished** mid-course: say nothing
  to the learner. If modules remain, relaunch with `startModule` = next undelivered.
- **Workflow tool unavailable or refused:** build every module inline, as in the
  single-agent version of this skill. Nothing else changes.
- **Course closes, early or on time:** write `STOP`.

---

## Builder protocol

You are the builder. You never talk to the learner, and your final text is a
return value nobody reads closely: one line. Everything you produce is a file in
`buildDir`.

**Once, at start:** read `skillDir/references/module-build.md` and
`skillDir/references/teaching-voice.md`, and load the `artifact-design` skill.
Write `builder.heartbeat`.

**For each module n, starting at `startModule`, in plan order:**

1. **Reread the plan.** The tutor updates it at every boundary. If module n is
   `kind: chat-tell` or already `delivered: yes`, skip it. If the plan says
   `status: closed`, or `STOP` exists, go to Exit.

2. **If `M<n>.brief.md` already exists**, skip the draft: build the finished module
   from plan plus brief and go to step 5.

3. **Draft.** Write `M<n>.draft.html` as a complete module per `module-build.md`,
   built from the plan and its first assumptions about this learner: the plan's
   domain, the hook attempt, and, as the assumed impasse, the most likely wall from
   the `Wrong models` list. Everything is real — figures computed, the instrument
   working, gate items and the end set fully written with per-option explanations,
   the handoff block with the boundary question verbatim. Wrap the parts the
   learner's answer is most likely to change in comment markers so they can be found
   and replaced:

   ```html
   <!-- CHIRON:opening -->  names the wall, quotes the learner  <!-- /CHIRON:opening -->
   <!-- CHIRON:scenario --> the opening scenario                <!-- /CHIRON:scenario -->
   <!-- CHIRON:end-set -->  the end-of-module set               <!-- /CHIRON:end-set -->
   ```

   Then write `M<n>.notes.md`: the impasse you assumed, the domain and examples you
   assumed, and anything else you guessed. Then write the marker `M<n>.drafted`.

   Format: `artifact-body` means no doctype, html, head or body tags; inline all CSS
   and JS; images as data: URIs, since an artifact page can't load other hosts.
   `full-html` means a complete self-contained document.

4. **Wait for the brief.** Loop in bash: check for `M<n>.brief.md` and for `STOP`
   every 10 s, touch `builder.heartbeat` every cycle, about 9 minutes per bash call,
   then call again. If `STOP` appears, go to Exit. If 45 minutes pass with no brief,
   write `builder.exited` containing `timeout waiting for M<n> brief` and stop.

5. **Finish.** Read the brief and the plan again. Revise the draft. Don't patch it
   blindly: the brief can change more than the marked slots.
   - Opening: their wall, in their words, quoted, and what this module gives them
     to get past it. Never "in this module we will."
   - `case: has-it`: compress. Cut the scaffolding, open on the instrument or the
     practice, keep the end set at full size.
   - `confront`: the section carrying that claim names the wrong model first and
     shows where it fails.
   - `distractors_add`: each one appears in the end set as a distractor with its own
     explanation.
   - `drop_from_draft`: remove anything built on those assumptions.
   - From M2 on: the interleaving from the plan's schedule, reformulated, with
     answers that differ from the original items (`module-build.md`).
   Write `M<n>.html`, then the marker `M<n>.done`.

6. **Next module.** Go straight to step 1 for n+1, so the next draft is being
   written while the learner works through this one.

**Exit:** write `builder.exited` with the reason, and return one line, e.g.
`built M1–M3` or `stopped at M2: STOP`.

**Rules:**

- Never write the plan, a brief, the deck or anything outside `buildDir`.
- Never publish, never call the Artifact tool, never message the user.
- A draft that's 90% right is the point. The tutor's brief exists to fix the other
  10%, so spend the waiting time on nothing; don't speculatively build variants.

# Chiron

A Claude skill that tutors a topic the way a good human tutor does. Named for the
centaur who taught Achilles, Hercules, and Asclepius.

You name something you want to understand. Chiron asks a question or two to find
the exact place you get stuck, then builds an interactive module that teaches that
and nothing else. Between modules it reads your answers in chat and responds to
what you actually wrote. You leave with:

- two or three modules built around where *you* got stuck, not a generic syllabus
- a flashcard deck with backs written in your own words
- at least one answer of your own, applying the idea to your situation, that got a
  real response rather than being matched against an answer key

## The one idea

**Elicit to the impasse. Then tell, composed. Then practice. Then apply.**

The questions up front aren't there to make you derive the material yourself. They
get you to the wall: the specific thing you can feel you're missing. Explanation
lands when it arrives at that wall and slides off when it arrives before it. Once
you're at the wall, more questions would just be holding back, so Chiron stops
asking and builds.

The teaching goes into an artifact because teaching in chat drifts. Each chat turn
gets pulled toward the last message, and before long the learner is steering. A
module is written in one pass with the whole argument in view. Chat is kept for
the two things only chat can do: finding where you're stuck, and reading an answer
nobody anticipated.

> **Composition goes in the artifact. Response goes in chat.**
> Gradable checks (multiple choice with misconception distractors, set-the-control,
> drag-to-label, ordering, numeric with tolerance) live in the module. Anything
> whose value is that someone *reads* it, like free recall, applying the idea to
> your own case, or card backs, happens in chat.

## How a course runs

```
hook scenario → your attempt           (→ one goal question, only if needed)
  → the whole course is planned before anything is taught
  → for each module:
      one or two questions to find where you're stuck
      → the module is built around that, in the same turn
      → you work the checks, then answer the closing question in chat
      → Chiron responds → card fronts → you write the backs → added to deck
      → next module
  → close: answer the opening question in full, then one new case
```

1. **The hook.** One concrete scenario ending in one question that asks for a
   prediction and a reason. It's picked so that each common wrong model gives a
   visibly different answer. Your attempt shows Chiron where you're starting from,
   and the close measures your progress against it.
2. **The plan.** Before any teaching, Chiron writes a course plan: your goal, the
   full answer to the hook broken into claims (each tagged `fact`, `rule` or
   `principle`), two or three modules, the wrong models to confront, every check,
   and the terms you need to leave able to produce. You see the module list as a
   short numbered table of contents.
3. **Each module.** At most two questions, then the build. How a module opens
   depends on the claim type:
   - A fact is told, never guessed at.
   - A rule gets a worked example, then a partly blanked version to finish, then a
     bare problem.
   - A principle gets elicited or told, then applied to a new case.
   - A misconception gets a case where the wrong model predicts wrong.
4. **The boundary.** Your answer to the module's closing question comes back to
   chat. Chiron responds to it, answers your questions, then gives you card fronts
   to fill in. It pushes back on substance only, never on wording.
5. **The close.** You answer the opening question in full, then try the same idea
   in an unfamiliar setting. You get one recommendation for what to learn next, not
   a menu.

## What the modules do

- **Open on your words.** A module opens by quoting what you said you were stuck
  on, never with "in this module we will…".
- **Use real figures.** Anything that plots data comes from computed output. A
  diagram that's only a sketch is labelled as one.
- **Build instruments.** If the idea has a quantity you can turn, the module gives
  you a control to move and asks you to predict before each move.
- **Ask for predictions first.** Sections about what happens when open with a
  prediction you commit to before reading.
- **Include checks.** Gate items sit between sections, and an end set of four to
  six mixed-format items closes the module. Every option explains why it's right
  or wrong.
- **Interleave.** From module 2 on, at least a third of the end set pulls from
  earlier modules. Those items are reworded so you have to decide which mechanism
  applies.
- **Keep the page still.** Answering a check never moves the page.
- **Hand you back to chat.** Every module ends with a one-sentence summary, a line
  on what comes next, and the closing question to answer in chat.

## The flashcard deck

`flashcards.html` is a single self-contained file that grows as the course goes.
Chiron writes the fronts and you write every back. There are four card types:
term, "X vs Y" distinction, "Why is it wrong to say: …", and perceptual (a pool of
images per category, never a single canonical one). It schedules reviews with
SM-2, a standard spaced-repetition method. It also has due-today, back-first,
filters, and keyboard grading (1–4, space to flip). Use its save button to download
a copy with your review progress. It needs no network and no framework, and it
works opened straight from disk.

## Teaching voice

These rules hold everywhere, in modules and in chat:

- Short sentences, second person, one claim per paragraph.
- Every term is defined in the sentence that introduces it, and Chiron answers in
  your vocabulary first.
- A concrete instance comes first, then the general rule, then the formal name.
- An analogy is used only to show structure. It's mapped explicitly, with a note on
  where it breaks.
- Feedback is about the work, never the person. No generic praise and no faked
  enthusiasm.
- The machinery stays out of sight. You never hear "impasse", "elicit" or "claim
  state".

## Repository layout

```
SKILL.md                      # the skill: flow, stop rules, boundary, close, failure modes
references/
├── teaching-voice.md         # register, jargon, analogies, feedback
├── plan-schema.md            # the course-plan file: claims, modules, wrong models, log
├── module-build.md           # module mechanics: checks, interleaving, instruments, handoff
├── flashcard-deck.md         # flashcards.html data format and required features
└── example-run.md            # an abridged course, turn by turn (exposure in photography)
```

`SKILL.md` is loaded when the skill is invoked. It tells Claude when to read each
reference file.

## Installing

Chiron is a standard Agent Skill: a folder with a `SKILL.md` and supporting files.

- **Claude Code:** clone or copy this repo into `~/.claude/skills/chiron-course-tutor/`
  (personal) or `.claude/skills/chiron-course-tutor/` (per project).
- **Claude apps:** zip the repo contents and upload them as a custom skill in
  Settings → Capabilities.

It works best where Claude can create artifacts, and it uses code execution, inline
widgets and memory when they're available. Where one is missing, Chiron does that
step another way instead of skipping it.

## Using it

Ask to be taught something properly:

- "Give me a crash course on options pricing."
- "I want to actually understand how transformers work."
- "Tutor me on exposure. My indoor shots keep coming out muddy."
- "Chiron: plate tectonics."

A plain question ("how does X work?") gets a plain answer, not a course, unless you
ask to be taught.

At any point you can say "just tell me" (stops the questions), "skip the cards", or
"I've got this" (Chiron wraps up early with the essentials).

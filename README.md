# Chiron

A Claude skill that tutors a topic the way a good human tutor does. Named for the
centaur who taught Achilles, Hercules, and Asclepius. This version is built for
Claude Cowork.

You name something you want to understand. Chiron starts building the course right
away and asks you a question while it works. You keep talking to it the whole time
it builds. It finds the exact place you get stuck, then fits an interactive module
to that and nothing else. Every check you answer in a module is logged, so when you
come back to chat it picks up from what you actually did. You leave with:

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
module is written with the whole argument in view. Chat is kept for the two things
only chat can do: finding where you're stuck, and reading an answer nobody
anticipated.

> **Composition goes in the artifact. Response goes in chat.**
> Gradable checks (multiple choice with misconception distractors, set-the-control,
> drag-to-label, ordering, numeric with tolerance) live in the module. Anything
> whose value is that someone *reads* it, like free recall, applying the idea to
> your own case, or card backs, happens in chat.

Two additions make the modules more than static pages:

- **Live grading.** When a module is published with the `sample` capability, a
  model can read free text inside it. Short "explain the mechanism" items can then
  live in the module, graded against a rubric written in the course plan. The
  module's closing question and the throughline still come back to chat, because
  they need the tutor's whole context.
- **The modules report back.** Every check you answer in a module (gates, end-of-
  module items, predictions, live-graded items) is logged to that module's
  database. Chiron reads the log at every boundary to see what you got right on
  the first try, what you missed, and which wrong models showed up. It uses that
  without naming it: you never hear "you picked B on question 4."

## How a course runs

```
your request
  → opening message: building now + Send now + the hook        (same turn:)
  → research, the course's look, draft module 1 from first assumptions
      ← your hook answer (Send now) → reply, goal question if needed
      → plan the whole course → module list + one question
      ← your answer → one more question at most
      → the wall → "That's where we'll start, give me a minute…"
  → module 1 fitted to the wall → published
  → you work through it                                   (every check logged)
  → your answer to its closing question
      → Chiron reads the log → responds → card widget
      → drafts the next module meanwhile
  → your card backs → feedback + revised cards, until every card is done → deck
      → next module: one question, fit, publish
  → close: answer the opening question in full, then one new case
```

1. **The opening message.** It goes out within seconds, before any research. One
   line saying the course is being built, one line on **Send now**, then the hook.
2. **The hook.** One concrete scenario ending in one question that asks for a
   prediction and a reason. It's picked so that each common wrong model gives a
   visibly different answer. Your attempt shows Chiron where you're starting from,
   and the close measures your progress against it.
3. **The plan.** Chiron writes a course plan: your goal, the full answer to the
   hook broken into claims (each tagged `fact`, `rule` or `principle`), two or
   three modules, the wrong models to confront, every check, and the terms you need
   to leave able to produce. You see the module list as a short numbered table of
   contents.
4. **Each module.** At most two questions, then the fit. How a module opens depends
   on the claim type:
   - A fact is told, never guessed at.
   - A rule gets a worked example, then a partly blanked version to finish, then a
     bare problem.
   - A principle gets elicited or told, then applied to a new case.
   - A misconception gets a case where the wrong model predicts wrong.
5. **The boundary.** Your answer to the module's closing question comes back to
   chat. Chiron checks it against the module's log, responds, answers your
   questions, then gives you cards to fill in. It pushes back on substance only,
   never on wording.
6. **The close.** You answer the opening question in full, then try the same idea
   in an unfamiliar setting. You get one recommendation for what to learn next, not
   a menu.

### Build while you talk

You never wait for the course to start. Chiron builds inside one long turn, and you
talk to it during that turn by typing a reply and pressing **Send now**. Your
message reaches it as soon as its current step finishes, so it keeps every step
short: a module is written section by section, one search per call, one figure per
call. Your message always comes before the build. When the conversation reaches
the wall, the draft is already there, so the only wait is fitting it to what you
said.

## What the modules do

- **Open on your words.** A module opens by quoting what you said you were stuck
  on, never with "in this module we will…".
- **Look like the subject.** Each course gets its own palette, fonts and one motif
  taken from the topic, e.g. sailcloth and sea for sailing, chalk on slate for math.
  Never the same dark-navy default.
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
  applies. A weak spot in the log overrides the schedule and gets pulled forward.
- **Keep the page still.** Answering a check never moves the page.
- **Hand you back to chat.** Every module ends with a one-sentence summary, a line
  on what comes next, and the closing question to answer in chat.

## Cards and the flashcard deck

At each boundary, Chiron shows a card widget inline in chat: one row of cards you
scroll sideways, with fronts it wrote and backs you fill in. There are four card
types: term, "X vs Y" distinction, "Why is it wrong to say: …", and perceptual (a
pool of images per category, never a single canonical one). You submit once, and
each card comes back marked:

- **Done:** correct and complete in substance. Wording and polish never count.
- **Close:** the right idea with one piece missing. One light nudge.
- **Revise:** wrong, missing the core, or restating the front. A question that
  points at the gap.

Chiron nudges with questions and never writes a back for you. A new widget comes
back with your text prefilled, and you revise until every card is done. The next
module waits on the cards; say "skip the cards" or "next" to move on anyway.

Finished cards go into `flashcards.html`, a single self-contained file that grows
as the course goes. It schedules reviews with SM-2, a standard spaced-repetition
method. It also has due-today, back-first, filters, and keyboard grading (1–4,
space to flip). Use its save button to download a copy with your review progress.
It needs no network and no framework, and it works opened straight from disk.

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
  state". Building the course and Send now are the only parts you hear about.

## Repository layout

```
SKILL.md                      # the whole skill, self-contained
references/                   # earlier standalone versions, not loaded by SKILL.md
├── teaching-voice.md         # register, jargon, analogies, feedback
├── plan-schema.md            # the course-plan file: claims, modules, wrong models, log
├── module-build.md           # module mechanics: checks, interleaving, instruments, handoff
├── flashcard-deck.md         # flashcards.html data format and required features
├── example-run.md            # an abridged course, turn by turn (exposure in photography)
└── background-builder.md     # an alternative two-agent design (not used by SKILL.md)
```

`SKILL.md` is one file. The flow, stop rules, boundary, close and failure modes come
first, followed by seven appendices it reads as it goes:

| Appendix | Covers |
|---|---|
| A | Teaching voice |
| B | Course plan schema |
| C | Building a module, including the look, live grading and the evidence log |
| D | Building while you talk |
| E | Flashcard deck |
| F | Example run |
| G | The card widget |

`SKILL.md` doesn't load the files in `references/`. They're earlier standalone
versions of the appendices, and the appendices are the current ones. For example,
`module-build.md` has no live grading or per-course look. `background-builder.md`
describes a version where a background agent builds the modules while the tutor
talks. The current skill does both itself in one turn (Appendix D).

During a course, Chiron keeps its working files in `chiron/<topic-slug>/` in the
working directory: the plan, research notes, the modules and the deck. If a folder
is connected, it mirrors the plan, the deck and each finished module to
`Chiron/<topic-slug>/` there.

## Installing

Chiron is a standard Agent Skill: a folder with a `SKILL.md` and supporting files.

- **Claude apps (Cowork):** zip the repo contents and upload them as a custom skill
  in Settings → Capabilities.
- **Claude Code:** clone or copy this repo into `~/.claude/skills/chiron-course-tutor/`
  (personal) or `.claude/skills/chiron-course-tutor/` (per project).

It's written for Cowork and leans on its tools: mid-turn messages (Send now), the
Artifact tool with the `sample`, `db` and `user` capabilities, artifact data for
the evidence log, and inline widgets for instruments and cards. Where one is
missing, Chiron does that step another way instead of skipping it:

| Missing | Fallback |
|---|---|
| Artifact publishing with capabilities | Modules are sent as files, with no logging or live grading |
| The evidence log | The boundary runs on your chat answers alone |
| Inline widgets | Instruments go in the next module; cards come as a numbered list |
| Tappable prompts | Lettered options in text |
| Image search | A schematic, labelled as one |
| A connected folder | Files stay in the session's outputs |

## Using it

Ask to be taught something properly:

- "Give me a crash course on options pricing."
- "I want to actually understand how transformers work."
- "Tutor me on exposure. My indoor shots keep coming out muddy."
- "Chiron: plate tectonics."

A plain question ("how does X work?") gets a plain answer, not a course, unless you
ask to be taught.

While Chiron is building, type your reply and press **Send now** so it gets there
right away instead of after the build finishes.

At any point you can say "just tell me" (stops the questions), "skip the cards" (if
you skip twice, Chiron stops offering them), or "I've got this" (Chiron wraps up
early with the essentials).

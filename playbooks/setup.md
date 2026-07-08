# Setup — Per-Course Onboarding

Runs once PER SUBJECT, not once per install. Creates the course folder,
curriculum, and per-course learner model. If no global profile exists
yet, creates that too.

---

## 1. INTERVIEW

Conversational, not a form. Two rounds.

**Round 1 — what and why**
- What do you want to learn?
- One-time overview or a course tracked over time? (Overview → route to
  bitesize.md instead. Stop here.)

**Round 2 — calibration**
- Background with this subject? (none / heard of it / working knowledge / solid)
- Tried learning it before? Where did it break down? (surfaces prior
  misconceptions — seed these into early sessions)
- What do you want to be able to DO at the end? (professional use /
  conceptual understanding / build something / pass an exam)
- How deep? (survey / working knowledge / expert)
- How do you like to be taught? (make me work for it / mixed / explain
  directly) — this sets the assistance-level PRIOR, not a fixed dial.

Listen fully before proceeding. If a global profile already exists,
skip questions it already answers; confirm rather than re-ask.

## 2. GENERATE THE CURRICULUM

Draft a topic sequence for this subject, goal, and depth. Tiers/phases
if the subject has natural structure, flat list if not. Every topic:
one line. Order by real prerequisite structure, not textbook convention.

Present conversationally: "Here's what I'm thinking — [framing]. What
feels off?" Adjust on feedback. Confirm explicitly before writing files.

## 3. SCAFFOLD THE VAULT (filesystem tier)

On confirmation, write:

1. `learner-profile.yaml` at vault root from templates/learner-profile.yaml
   — ONLY if it doesn't exist. Never overwrite an existing profile.
2. `courses/<subject>/course-index.md` from templates/course-index.md —
   the confirmed curriculum with all topics status: not-started.
3. `courses/<subject>/learner-model.yaml` from templates/learner-model.yaml
   — empty mastery, misconceptions seeded from "where it broke down"
   answers if any.
4. `courses/<subject>/topics/<n.n-slug>.md` from templates/topic-stub.md,
   one per topic, prerequisites wikilinked. These are the domain-model
   graph nodes — metadata only, no prose.
5. `courses/<subject>/worksheets/` and `concepts/` as empty folders.
6. `flashcards.html` at vault root from templates/flashcards.html if
   absent.

Memory tier: write profile + curriculum + traversal state to Claude's
memory instead. Say plainly that notes/decks require the filesystem tier.

## 4. OFFER PROJECT INSTRUCTIONS

Offer a paste-ready project instructions block: subject, goal, depth,
the confirmed curriculum, and "follow the Chiron session cycle; read
the learner model at session start." Keep it under ~40 lines.

## 5. BEGIN

"Ready? Name a topic or ask what to start with." If asked, recommend
the true entry node with one sentence of rationale.

---

**Anti-patterns**: interviewing for 10 questions when 5 answers exist in
the profile; generating a 60-topic curriculum for a "survey" depth;
writing any file before explicit curriculum confirmation; re-onboarding
a learner who already has a global profile.

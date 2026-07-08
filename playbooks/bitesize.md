# Bitesize — Single-Chat Quick Lessons

The front door. No onboarding, no profile required, works in every tier.
Explanation-forward (most bitesize learners are cold on the topic —
assistance high by default), but generation is still mandatory. A learner
must not be able to get through a bitesize lesson passively.

Target: 10–20 minutes of engagement. Anything genuinely bigger → say so
and offer a course (setup.md) or split the topic.

---

## 1. FAST CALIBRATION (silent, from the query itself)

The message is the profile. Read register, vocabulary, and what they
got right/wrong in their own framing:
- Technical term used correctly → they own it; use it back.
- Plain-language mechanism description → intuitive reasoner, no formal
  background; explain in their register, introduce a term only when
  load-bearing.
- A stated wrong model ("shouldn't they displace the same?") → the
  lesson is a repair job: target THE gap, don't re-teach the field.
- Stakes markers ("before my exam", "for work tomorrow") → compress.

Competence check before committing (SKILL.md rule 5): if the topic is
safety-critical, embodied, fast-moving, or hallucination-risky, do NOT
fake the lesson. Name a real resource, bracket it, offer study-buddy.

## 2. THE LESSON

Two delivery modes; pick by size and manipulability:

**Conversational** (default for narrow gaps): hook → tight explanation
chunks with a prediction or retrieval demand between chunks → transfer
check. All register rules apply. For a single misconception, the whole
lesson may be four sentences plus one check — length scales to gap size.

**Artifact** (for topics with real structure or a manipulable mechanism):
a single self-contained HTML lesson. Read references/visual-artifacts.md
first. Structure, in order:

1. **Hook** — one scenario or counterintuitive fact seeding the mechanism.
2. **Instruction segments** — one concept each, learner-paced, concrete
   example before abstraction. Mayer governs form: no decoration
   (coherence), cue attention (signaling), no narrating on-screen text
   (redundancy), chunked (segmenting).
3. **Predict-before-reveal** — before ANY interactive element
   demonstrates behavior, the learner commits to a prediction. The sim
   does not move until they have. Highest-leverage component in the
   artifact.
4. **Sim** — only if manipulating the mechanism produces insight reading
   can't. If the sim would be an animation of the text, cut it.
5. **Distributed checks** — retrieval through the lesson, never dumped
   at the end. Constructed response over recognition wherever the format
   allows. Minimum: one recall mid-lesson, one transfer item at close
   using a scenario the lesson has NOT shown. Feedback is elaborated —
   why, not just right/wrong — and appears only after commitment.
6. **Close** — one sentence: what you can now do. Missed items → offer
   flashcards. If a better canonical resource exists → surface it with
   a two-sentence bracket (what to extract, what to skip).

## 3. GENERATION RULESET (binding for every artifact)

- Prediction precedes every reveal. No exceptions.
- No check is skippable by scrolling; reveal-gated progression.
- At least one item is transfer, not recall.
- Recognition MCQ is the weakest allowed form — never the only form.
- Feedback elaborates the why and appears after commitment, not before.
- Concrete → abstract ordering inside every segment.
- One concept per segment; coupled concepts get two segments plus an
  explicit bridge.
- All CSS/JS inline; cdnjs only; light mode enforced in CSS regardless
  of system setting.

## 4. STATE TOUCH

- Profile exists (filesystem tier): one line in the course-adjacent
  model or profile — "bitesize touch: <topic>, <date>". NOT a mastery
  record. Offer missed-item cards into flashcards.html.
- No profile: fully stateless. Offer a standalone deck artifact for
  missed items. Soft close: "Want this tracked over time? I can set up
  a course." Route yes → setup.md. Never push twice.

---

**Anti-patterns**: explainer bloat (re-confirming correct understanding
at length, closing summaries, unrequested tangents); quiz stapled at the
end; sims as decoration; recognition-only checks; introducing formal
notation to a plain-language asker; faking coverage of a topic past
Chiron's reliable competence; upgrade-nagging.

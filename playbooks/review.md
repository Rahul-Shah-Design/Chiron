# Review — Spaced + Interleaved Retrieval

[Phase 2 playbook — depends on learner-model review queue and
flashcards.html being populated by session/feedback cycles.]

Chiron teaches spacing; this is where it practices it. Runs on "review",
"drill me", "what's due", or when recommend.md flags staleness.

---

## 1. BUILD THE SET

Read learner-model.yaml: review queue, last-touched dates, mastery
signals, misconception ledger. Select due topics — stale-strong topics
decay too; they belong in the set, not just weak ones. 2–4 topics per
session. INTERLEAVE: shuffle items across topics rather than blocking
by topic — discrimination between concepts is the point of mixing.

Memory tier: approximate from memory's last-touched notes; say plainly
the scheduling is rough in this tier.

## 2. RUN RETRIEVAL

- Free recall before cued recall: "walk me through X" before any
  fill-in prompt.
- **Mutate application scenarios.** An application item must present a
  fresh surface each exposure or it collapses into recognition of the
  remembered answer. Same mechanism, new skin, every time.
- Unresolved ledger misconceptions get one item each, deliberately.
- One item at a time. Feedback after commitment, elaborated, per
  feedback-principles.md.
- Register rules apply. No re-teaching mid-review; a failed item gets
  one targeted exchange max, then flagged.

## 3. SCORE ON MORE THAN CORRECTNESS

Update scheduling signal from correctness AND retrieval quality:
latency, hesitation, precision of mechanism language. Slow shaky
correct ≠ fast clean correct. Downgrade stale-strong ratings the
evidence no longer supports — mastery records must reflect current
state, not historical peak.

## 4. STATE WRITE

- learner-model.yaml: per-topic review outcomes, next-due adjustments
  (expand interval on clean retrieval, contract on shaky), resolved or
  persisting misconceptions, evidence type: review.
- New cards for items that failed → flashcards.html, learner's wording.
- Sync any card outcomes the learner reports from browser deck drills
  if they mention them.

## 5. CLOSE

One line: what's solid, what's contracting. If a topic failed hard,
recommend a targeted re-session rather than pretending review fixed it.

---

**Anti-patterns**: blocking by topic; identical application items across
exposures; re-teaching inside review; scheduling from correctness alone;
letting stale "strong" ratings stand unexamined; review sessions longer
than ~20 minutes.

# flashcards.html

One self-contained HTML artifact per course, named `flashcards.html`. Read this
before the first write; after that, only edit the card array. If the artifact
already exists in this conversation, update it in place by appending to the data
block; never regenerate the file from scratch.

## Data

Cards live in a single `<script id="cards" type="application/json">` block so
appending is a text edit, not a rebuild:

```json
{ "id": "ai-adaptive-learning/2.5/003",
  "course": "ai-adaptive-learning", "module": "M2", "topic": "2.5 BKT",
  "type": "term | distinction | misconception | perceptual",
  "front": "…", "back": "…",
  "images": [],
  "created": "2026-09-04",
  "sm2": { "ease": 2.5, "interval": 0, "reps": 0, "due": "2026-09-04" } }
```

`back` is the learner's words, verbatim. Chiron never edits it. If a back was
corrected during the boundary, the corrected version is what they typed after
the push, not Chiron's phrasing.

Card types and what the front looks like:

- **term** — the term alone. Back: their definition.
- **distinction** — "X vs Y". Back: the difference, in their words.
- **misconception** — "Why is it wrong to say: <their wrong belief>". Back:
  their corrected reasoning. Created only from the plan's misconception ledger.
- **perceptual** — an exemplar pool, not one picture. `images` holds three or
  more URLs for the category; the front shows one drawn at random each review,
  and the deck interleaves perceptual cards across categories. Back: the
  learner's distinguishing features. Never one canonical image per category;
  that is recognition of the photo.

Not a card type: "what is <principle>". Principles are checked by application,
in chat.

## Required features

- SM-2 scheduling, per card, state held in memory during a review session and
  written back into the JSON block on save.
- Due-today view as the default. Count of due cards visible before starting.
- Show back-first mode (front hidden, recall the term from the definition).
- Shuffle within the due set.
- Filter by course, by module, by type. Sort by due date or by creation.
- Grade buttons: again / hard / good / easy. Keyboard: 1–4, space to flip.
- Perceptual cards draw a random image from `images` on each show.
- A save button that downloads the deck as `flashcards.html` with the updated
  JSON block, so the learner can keep a copy that remembers their scheduling.
  Say this once in the deck's header.
- A reset-scheduling button, per filter.
- No frameworks, no CDN, no network. Inline CSS and JS. Works as an artifact and
  as a downloaded file opened from disk.
- Readable at 380px wide.

## Appending

At a boundary, after the learner has filled the fronts and Chiron has pushed on
any substantive errors: append the new card objects to the JSON array, update
nothing else in the artifact, and say in one line how many cards were added. No summary of the cards; they wrote them.

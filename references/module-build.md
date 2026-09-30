# Building a module

Read before building any module. Mechanics only; what the module contains comes from the plan.

One artifact per module, built in one pass, planned sections in planned order.

**It must read as authored.** A title, a stated arc, sections that follow from each
other, and a closing line that folds the whole module back into one sentence. Each
section: the claim, the explanation, the figure or instrument beside it, and a line
telling the learner what to notice. Open by placing the module in the course, and
call back to earlier modules by name where the plan says a claim rests on one.

**Build it in the learner's world.** Their domain from the plan, their metrics, their
examples. This is what separates a course from a textbook chapter.

**Figures show real output.** If a figure plots data, compute the data — run the
code, use the real numbers. A scree plot with an invented elbow, a payoff curve with
guessed numbers, a distribution drawn by hand: these are worse than no figure,
because they teach a shape that isn't true. Where a figure is a schematic rather than
data, that is fine; label it as one. **SVG is for shapes a child could draw** — the
moment accuracy depends on the drawing, use computed output or a schematic.

**Consult the frontend-design skill for the visual direction** before writing the
markup. The course should look like it was designed for this subject.

### Predict first

Where a section's claim is a behaviour — what happens when — open it with a
prediction: two or three options, commit, no grading, no reveal. The section that
follows is the feedback. A wrong guess corrected by the next paragraph sticks harder
than a right answer read cold, and a learner who has committed reads to find out
rather than to get through. Definitional claims get no prediction; there is nothing
to be wrong about yet. The hook is this move at course scale; this is it at section
scale.

### Procedures

A `rule` claim — steps the learner must carry out — does not get an instrument by
default, it gets this sequence: one example worked through in full with the reason
for each step stated, then a gate that is the same procedure with the last steps
blanked for the learner to finish, then the bare problem in the end-of-module set.
Reading a full solution is where a novice learns the procedure; solving unaided is
where they show it; the blanked version carries them from one to the other. Never go
from the rule straight to a bare problem.

### Checks are not optional and they are not decoration

Every module carries gradable checks in two places. A module that ships without them
is an explainer, not a course.

**Gate items, between sections.** One check standing between the learner and the next
section, at each section boundary the plan marks. Gating is legitimate here in a way
it never is in chat: the artifact is a place the learner moves through, and a check
they must clear is pacing. Every gate must be passable from what the module has
already delivered. Clearing it means answering and reading the per-option
explanation, not answering correctly: once the explanation is read, a retry is
recognition of the explanation, so there is no retry loop and no lock. The gate paces
and diagnoses; it does not hold anyone.

**An end-of-module set, four to six items**, sitting after the last section and
before the handoff block. This is the real one. Mix the formats — multiple choice
with misconception distractors, set-the-control-until-the-output-flips, order-these-
steps, drag-to-label, numeric entry with a tolerance — because each tests something
different, and a set of six MCQs tests recognition six times.

**Every item is graded with a per-option explanation.** A wrong answer says why *that*
answer was wrong. That is the entire value of misconception distractors, and an item
that only says "incorrect" has thrown it away. Options are parallel in length and
specificity: the longest, most hedged option is the correct one often enough that
learners have learned to pick it. Never a free-text box with a canned resolution.

**Never place an item directly beneath the section that answered it.** A check under
its own explanation is recognition in a card's costume. Lag it by at least one
section; the end-of-module set does this for free.

### Interleave: pull earlier modules forward

From module 2 onward, **at least a third of the end-of-module set comes from earlier
modules**, drawn from the schedule written in the plan. In a six-item set that is two
items; in a four-item set, at least one, and prefer two.

This is the desirable-difficulty move, and it is the whole reason to build a course in
modules rather than one artifact. Blocked practice — every item testing the thing you
just read — produces fluent performance in the moment and poor retention afterward.
Interleaved items feel harder, and the learner will get more of them wrong. That is
the mechanism working, not a sign the course is failing.

A pulled-forward item earns its place when the learner has to decide *which*
mechanism applies — the earlier module's or this one's — before applying it. An item
that only asks them to recall module 1 is spaced retrieval, which is worth having. An
item where module 1's answer and this module's answer are both plausible and only one
is right is interleaving, which is worth more, because telling similar things apart
is what blocked practice never trains. At least one pulled-forward item per set is
the second kind.

Interleaved items must be **reformulated, not repeated.** The same question a second
time tests whether they remember answering it. Change the surface: a different
scenario, different numbers, the same underlying discrimination. An item that asked
which control adds real photons should come back as a situation where they must decide
which control to move and why.

**The test: set the earlier item's correct answer beside the new one. If they are the
same thing — the same number, the same option, the same ordering — you have reissued the
item, whatever changed in the stem.** A new format wrapped around an unchanged answer is
the same recognition in a new costume. Writing *reformulated* in the plan is not doing
it; the answers have to differ.

Say nothing to the learner about interleaving. Do not apologise for the difficulty, do
not flag which items are callbacks, do not soften the set.

### The check interaction must not move the page

Feedback appears where the learner is already looking, and the page does not move
when they answer. Getting this wrong makes the learner scroll back up to read their
own result, every single time.

- **Never call `scrollIntoView()`, set `location.hash`, or call `focus()` without
  `{preventScroll: true}`** in response to a selection. If focus must move for
  accessibility, move it without scrolling.
- **Reserve the feedback space before it is filled.** The explanation block sits in
  the DOM at its full height from the start, hidden with `visibility: hidden` or
  `opacity: 0` — not `display: none`, which reflows everything below it on reveal and
  shifts the option the learner just clicked out from under their cursor.
- **Unlocking the next section does not scroll to it.** Reveal it and let the learner
  scroll. If the module uses a continue button, that click is an explicit action and
  may scroll.
- **No `scroll-behavior: smooth` on the root element.** Combined with any anchor jump
  it produces a slow ride nobody asked for.
- Test the sequence mentally before shipping: click an option near the bottom of the
  viewport, and the explanation must be readable without touching the scrollbar.

### Instruments

**When a claim's mechanism has a quantity the learner can turn, build the
instrument and let the prose shrink to what to move and what to watch.** A price, a
rate, a weight, a probability, a threshold, a count, an angle. Arithmetic worked out
in prose — *ears × 8 + fur × 1 = 73* — is an instrument that didn't get built.

An instrument is delivered as a section, in this order: the claim in one sentence;
the instrument; a guided experiment of two or three steps, each with a prediction
before the move (*say what Y will do, then do X and watch Y; now do Z — what flips?*);
and the gradable check on it (*set both weights equal — what
does the output become?*). A bare widget between two paragraphs teaches nothing,
because nothing told the learner what it was for.

It is an instrument only if you can name the control, the variable it changes, and
what visibly changes. If you can't name all three, it is a diagram — label it one.
Not a quota: a claim with nothing to vary gets no instrument, and forcing a control
onto one is sim theater. But a module on a mechanism that has variables and ships
with none has under-built.

### Every module ends with a handoff

**A module never ends on its last paragraph.** The final thing in the artifact is a
closing block, visually set apart from the content, that hands the learner back to
the conversation. Without it the learner finishes reading and sits there — the course
has no exit.

The block contains, in this order:

1. **The fold-back** — the module's whole argument in one sentence.
2. **One line on what the next module does**, tied to the throughline, so finishing
   feels like arriving somewhere rather than stopping.
3. **The boundary question itself, printed in full**, with a plain instruction to
   answer it back in the chat. Something like: *Head back to the chat and answer
   this before module 3 — [question].*

**No input box, no submit button, no "check my answer."** The artifact cannot read
free text. The block sends them to the place that can.

This also means the learner's next message *is* their answer to the boundary
question, which removes two round trips from the loop: they no longer have to say
"done," and you no longer have to ask the question separately.

The closing block comes after the end-of-module check set, and if the module is gated
section by section it is revealed only once that set is done.

**Deliver the artifact, then stop.** One short message: what the module covers, and
that the question they need to answer is at the end of it. No summary of the content
— they are about to read it. No question in the message; the question is in the block.

# Retrospective — design-from-docs Session

**Date:** 2026-02-20
**Topic:** pencil.dev design-from-docs session — scope creep, token usage, and session limit hit
**Source:** Conversation summary (compacted) + thinking log at `design-logs.txt`

---

## Problems and Difficulties

- **Scope was never estimated before execution began.** The skill parsed the cases and immediately started designing. The agent only realized "this is ~40-60 screens, 35+ batch calls" mid-setup — after variables were set and the first screen was underway. By then it was too late to negotiate. The user had to manually stop the session when context ran out, with 1 of 5 cases completed.

- **No scope-and-approach checkpoint after estimation.** Even if scope had been estimated, there was no step where the agent would surface it to the user and propose options: "do one case at a time with approval in between", "limit to N cases per session", "do wireframe-only first pass", etc. The skill went straight from setup to full execution with no exit ramp.

- **Visual direction was committed to without user validation.** The user said "feel like Linear" (refined, polished dark UI). The agent selected a terminal/monospace style guide, internally noted the tension, and proceeded anyway — never producing a visual sample for the user to react to. The session ran on a potentially wrong aesthetic for its entire duration. For any project with no existing design system, a small proof-of-concept screen (basic nav, cards, typography, buttons) should be shown to the user for approval before full design work begins.

- **Style guide mismatch was never discussed.** Beyond the proof-of-concept issue above, when the selected style guide conflicts with the user's stated reference app, this should be raised as an explicit question before any canvas work is done.

- **~1,700 lines of planning before the first design call.** The thinking log is dominated by circular deliberation about screen dimensions (900px? 1100px? 720px?), layout strategy, and ops budgeting — repeatedly arriving at and abandoning the same conclusions without converging. This burned significant tokens that could have gone toward actual design output.

- **Pencil.dev API quirks caused repeated wasted calls.** Three `set_variables` calls failed before the correct format (`{type, value}`) was discovered. The stroke format (`thickness:{all:1}` silently maps to 0), `fill_container` inside `layout:none`, and missing `layout:"horizontal"` on screen frames were each hit multiple times — each requiring a correction batch call consuming the 25-op budget.

- **Layout bugs discovered only after execution.** Missing `layout:"horizontal"` on screen frames was the most common error — it happened across nearly every new screen and required a separate fix batch each time. No pre-flight checklist existed to catch these before submitting.

- **Canvas label text overlaps the frame title in pencil.dev.** When placing a text label above a screen frame (e.g., the step description "1. App opens — 8:45am"), pencil.dev renders its own frame name as a title that visually overlaps with the label text. Not enough vertical margin was left between the label and the frame edge, causing the label to collide with the frame's built-in title. Labels need at least 20px more clearance — either extra bottom margin on the text node or extra top margin on the frame.

- **Interaction annotations between screens were hard to read.** The arrow + explanation text (e.g., "→ clicks this button") placed in the gap between screen frames used a font size that was too large to fit the 80px horizontal gap comfortably, and the text did not wrap. The annotations overflowed or were truncated.

---

## Improvement Suggestions

- **Add a mandatory scope estimation step to the skill.** After parsing cases and variations, calculate the total screen count (cases × variations × screens_per_flow), estimate batch calls needed, and present this to the user before touching the canvas. Example: "This scope is ~50 screens across 5 cases — roughly 100 batch calls, likely 2-3 sessions. How would you like to proceed?"

- **Add a scope-and-approach checkpoint with explicit options.** After estimation, pause and offer concrete options: (a) one case per session with approval to continue, (b) wireframe-only first pass for all cases, (c) full fidelity but limit to N cases now. Do not start designing until the user picks an approach.

- **Draw a proof-of-concept screen before starting the full design.** When there is no existing design system or design file to reference, produce a single minimal sample screen (basic nav, a card, some text, a button) in the chosen style and show it to the user for visual direction approval. Only proceed with full case flows after the user confirms "yes, this is the look I want."

- **Discuss style guide conflicts with the user before committing.** When the selected style guide conflicts with the user's reference app (e.g., terminal vs. Linear), surface it explicitly: "The closest style guide is terminal-inspired. Linear uses a more refined aesthetic. Should I adapt it?" Do not proceed on a potentially wrong aesthetic.

- **Record pencil.dev API constraints as persistent memory.** The variable format, stroke format, and fill_container behavior are stable quirks that should not be re-discovered each session. Write them to a memory file so they are available in future sessions without trial and error.

- **Use a pre-flight checklist before each screen batch.** Before submitting a `batch_design` call that creates a new screen frame, verify: (a) `layout:"horizontal"` is set, (b) all blocks inside `layout:none` use explicit pixel widths, (c) strokes use `thickness:1` not `{all:1}`. This eliminates the most common fix-batch class.

- **Add 20px+ clearance between canvas label text and screen frames.** Position step label text at least 20px above the top edge of its screen frame (or leave 20px extra margin below the text) to avoid overlap with the frame's built-in name title rendered by pencil.dev.

- **Use small font and wrapping for inter-frame annotations.** The arrow and interaction label between screens (e.g., "→ drags to grid") must fit within the horizontal gap between frames (80px at 720px screen width with 800px spacing). Use font size 8–9px and set a fixed width equal to the gap (80px) so the text wraps rather than overflows.

- **Take a screenshot after each completed screen.** The session took screenshots infrequently. Catching layout issues immediately after each screen — rather than after several — would have prevented cascading fix batches.

---

## Kaizen Actions

| What to update | Action | Priority |
|---|---|---|
| `design-from-docs` skill | Add scope estimation step: count total screens, estimate batch calls, present to user before any canvas work | High |
| `design-from-docs` skill | Add scope-and-approach checkpoint: offer "one case at a time with approval", "wireframe first pass", or "limit to N cases" before starting | High |
| `design-from-docs` skill | Add proof-of-concept step: when no design system exists, draw one minimal sample screen and get visual direction approval before full case flows | High |
| `design-from-docs` skill | Add style guide review step: if user's reference app and selected style guide conflict, ask the user to resolve before designing | High |
| `design-from-docs` skill | Canvas layout rule: add 20px+ clearance between step label text nodes and screen frame top edges | Medium |
| `design-from-docs` skill | Inter-frame annotation rule: use font size 8–9px with explicit width = gap width (e.g., 80px) so arrow labels wrap instead of overflow | Medium |
| `design-from-docs` skill | Add pre-flight checklist: `layout:"horizontal"`, explicit px widths in `layout:none`, `thickness:1` not `{all:1}` | Medium |
| New `pencil` skill + `design-from-docs` skill | Create a global `pencil` skill (auto-triggers on any pencil.dev mention) containing all API quirks and canvas rules; add Step 0 to `design-from-docs` to invoke it before opening any .pen file | Medium |
| `design-from-docs` skill | Add step: take a screenshot after each completed screen and verify before moving to the next | Low |

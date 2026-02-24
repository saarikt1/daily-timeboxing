# Design: Planning the Workday

**Date:** 2026-02-24
**Use case:** `Design/Use-cases.md` — "Planning the workday"
**Status:** Approved

---

## Overview

The planning view is a persistent two-panel layout within the day view. No wizard or modal — everything happens spatially. Tommi sees his tasks on the left and his timeline on the right, drags tasks onto the grid, and plans his day in one glance.

---

## Layout

### Overall structure

- **Left panel (~30% width):** Task panel for planning. Collapses to an icon rail during work/focus mode.
- **Right panel (~70% width):** Day timeline. Scrollable time grid (e.g. 7am–8pm). Google Calendar events visible immediately as non-draggable overlays.
- **Top bar:** Date, focus mode toggle (collapses left panel), dark mode toggle, user avatar/settings.

### Left panel — three vertical zones

1. **Weekly objective** — single-line text field at the top. Quiet placeholder if empty ("Add a weekly goal if you want"). Resets each Monday. Not dominant — present but unobtrusive.
2. **Carried-over tasks** — incomplete tasks from previous days, surfaced automatically under a subtle "From yesterday" label. Shown at the top of the task list so they're not missed.
3. **Inbox / today's tasks** — tasks flagged for today or from inbox. Quick-add input field at the bottom for capturing new tasks without leaving the view (Enter to submit, lands at bottom of inbox section with default 1h estimate).

Each task card shows: title, project color dot, and estimate pill (`1h`, `30m`, etc.). Tasks without an estimate show `1h` in a muted style (the default).

The left panel scrolls independently of the timeline.

---

## Scheduling Interaction

### Dragging a task to the timeline

- Drag a task card from the left panel onto the timeline.
- Block snaps to 10-minute increments and expands to fill the task's estimated duration (default 1h if no estimate).
- The task card in the left panel gets a "scheduled" visual treatment (muted appearance) but stays in the list until completed.

### Resizing a time block

- Drag the bottom edge of a placed block to adjust duration.
- Snaps to 10-minute increments.
- Updates the task's `estimate_minutes` in real time — the estimate pill in the left panel updates to match.

### Google Calendar events

- Shown as visually distinct (different color/texture), non-draggable blocks on the timeline.
- Visible immediately when the view loads — Tommi sees real commitments before placing any tasks.
- Used passively to inform planning; no interaction needed.

### Quick-add field

- At the bottom of the left panel.
- Type task title → Enter → task appears in inbox section with default 1h estimate.
- Can be dragged to the timeline immediately.

---

## Focus Mode

- A toggle in the top bar collapses the left panel to a narrow icon rail.
- The timeline expands to fill the full width.
- Scheduled time blocks remain visible; unscheduled tasks are hidden.
- Toggling back restores the full planning view.
- Designed for when Tommi shifts from planning to working.

---

## Handling Variations

**Client project with imposed deadlines:** Tasks show project color dots and labels. Tommi identifies priority tasks visually and drags those to the timeline first. No special UI needed beyond existing project coloring.

**Large backlog (9+ tasks, Monday after long weekend):** The left panel scrolls. Tasks that don't fit today simply remain unscheduled — nothing forces Tommi to place everything. No overbooking warnings in v1.

---

## Explicit Non-Goals (v1)

- No auto-scheduling or AI suggestions
- No overbooking warnings
- No wizard steps or prompts
- No write-back to Google Calendar

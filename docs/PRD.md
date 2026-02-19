# Daily Timeboxing App — Product Requirements Document

**Date:** 2026-02-19
**Status:** Draft
**Author:** Tommi

---

## Overview

A personal productivity web app focused on the daily timeboxing workflow. The core loop: capture tasks quickly, plan the day by dragging tasks onto a time grid, work with an active task timer, and review progress. Google Calendar events are shown read-only to inform daily planning. Data syncs across devices via Supabase.

---

## Goals

- Make it fast to capture tasks without breaking flow
- Make daily planning frictionless: see the calendar, drag tasks into slots, resize to fit
- Track time spent vs estimated to build better time intuition
- Keep the app usable on narrow desktop windows (quarter/half screen)
- Work across multiple laptops via cloud sync

---

## Non-Goals (v1)

- Mobile-first or touch-optimized experience
- Writing back to Google Calendar
- AI-powered auto-scheduling
- Collaboration or sharing with others
- Native desktop app (PWA is sufficient)

---

## Users

Single-user personal productivity tool. Work and home separation is handled by using separate accounts — no in-app mode switching needed.

---

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | React + TypeScript |
| Build tool | Vite |
| Styling | TailwindCSS + shadcn/ui |
| Calendar / drag-drop | FullCalendar or @dnd-kit |
| Backend / auth | Supabase (Postgres + realtime + Auth) |
| Hosting | Vercel |
| Offline / PWA | Vite PWA plugin |

---

## Data Model

### Task
| Field | Type | Notes |
|---|---|---|
| id | uuid | PK |
| user_id | uuid | FK → auth.users |
| project_id | uuid? | nullable for inbox tasks |
| parent_task_id | uuid? | nullable; set for subtasks |
| title | text | |
| notes | text | markdown supported |
| status | enum | `inbox \| todo \| done \| cancelled` |
| estimate_minutes | int | multiples of 10 |
| scheduled_date | date? | the day the task is planned for |
| order | int | sort order within project or date |
| is_recurring | bool | |
| recurrence_rule | text? | iCal RRULE string |
| created_at | timestamptz | |
| updated_at | timestamptz | |

### TimeBlock
Represents a task placed on the time grid (drag from task list to calendar).

| Field | Type | Notes |
|---|---|---|
| id | uuid | PK |
| user_id | uuid | FK |
| task_id | uuid | FK → Task |
| date | date | |
| start_at | timestamptz | |
| end_at | timestamptz | |

Resizing a time block (like a Google Cal event) updates `end_at` and syncs `estimate_minutes` on the task snapped to 10-min increments.

### TaskTimer
One row per start/stop/pause session.

| Field | Type | Notes |
|---|---|---|
| id | uuid | PK |
| user_id | uuid | FK |
| task_id | uuid | FK → Task |
| started_at | timestamptz | |
| ended_at | timestamptz? | null if currently running |
| duration_seconds | int? | set on stop |

### Project
| Field | Type | Notes |
|---|---|---|
| id | uuid | PK |
| user_id | uuid | FK |
| name | text | |
| color | text | hex color |
| archived | bool | |

### WeeklyObjective
| Field | Type | Notes |
|---|---|---|
| id | uuid | PK |
| user_id | uuid | FK |
| week_start | date | Monday of the week |
| title | text | |
| done | bool | |

### Google Calendar Events
Read-only, fetched from Google Calendar API and cached in memory (not persisted to Supabase). Shown as non-interactive overlay events on the time grid.

---

## Features

### Task Management

**Inbox**
All newly created tasks land in the Inbox unless explicitly assigned to a project. The inbox is a flat, unordered list for brain-dumping without friction.

**Projects**
Tasks can be organized into named, color-coded projects. Subtasks are supported one level deep (a task can have child tasks via `parent_task_id`). Each task can have a markdown notes field.

**Recurring Tasks**
Tasks can repeat on a schedule defined by an iCal RRULE string (daily, weekly, custom). Each occurrence is a separate task row generated from the rule.

### Quick Task Capture

Global keyboard shortcut (`Cmd+K` on macOS / `Ctrl+K` on Windows) opens a quick-add modal from anywhere in the app. Input: title (required), project (optional), estimate (optional, defaults to 30 min). On submit, the task is saved to Inbox or the selected project.

### Calendar & Timeboxing

**Day View**
- Scrollable time grid (configurable start/end hours, e.g. 7am–8pm)
- Google Calendar events shown as read-only blocks with distinct styling
- Unscheduled tasks listed in a panel alongside the grid
- Drag a task from the list onto the grid to create a TimeBlock
- Drag to move an existing TimeBlock to a new time slot
- Drag the bottom edge to resize (duration snaps to 10-min blocks)
- Resizing updates both the TimeBlock `end_at` and the task's `estimate_minutes`

**Week View**
- Overview of the full week
- TimeBlocks visible per day
- Google Calendar events shown
- Useful for planning ahead and reviewing load

### Task Timer

Active task bar shown at the bottom (or top) of the screen when a timer is running.

Controls:
- **Start** — begins a new TaskTimer session
- **Pause** — saves elapsed time, marks session ended
- **Resume** — starts a new session for the same task
- **Complete** — stops timer and marks task as done

Multiple timer sessions per task are recorded. Total time = sum of all `duration_seconds` for the task.

### Daily Planning Ritual

Triggered manually at the start of the day (or via a prompt if the day view is opened without any TimeBlocks scheduled).

Steps:
1. Review weekly objectives
2. Review yesterday's unfinished tasks (carry forward or drop)
3. Review today's Google Calendar events to see available time slots
4. Drag tasks from the list onto today's time grid

### Daily Shutdown Ritual

Triggered manually at the end of the day.

Steps:
1. Mark remaining tasks as done, deferred, or cancelled
2. Review actual time spent vs estimates
3. Set a brief intention or note for tomorrow (stored as a text field on the day)

### Weekly Objectives

A simple list of goals for the week, stored per `week_start` date. Shown in the sidebar and during the daily planning ritual. Not linked to individual tasks.

### Focus Mode

Filters the Today view to show only today's scheduled TimeBlocks. Hides the sidebar and task list. Useful when working in a quarter-screen window alongside other apps.

### Time Estimates

Tasks have an `estimate_minutes` field in multiples of 10. When a task is dragged onto the time grid, the block is sized to match the estimate. Resizing the block updates the estimate. The estimate is used in reports to compare planned vs actual.

### Time Tracking & Reports

**Daily report** — for a given date:
- Total estimated time (sum of TimeBlock durations)
- Total time spent (sum of TaskTimer durations)
- Per-task breakdown: estimated vs actual, variance

**Weekly report** — for a given week:
- Tasks completed
- Time estimated vs time spent
- Google Calendar time (meetings/events)
- Remaining/unscheduled time

### Google Calendar Integration

- Read-only OAuth integration (Google Calendar API)
- User connects one or more Google calendars in Settings
- Events fetched and cached in memory on app load / day change
- Displayed as non-draggable overlay events on the time grid
- Used during daily planning to see real commitments and available slots
- No write-back to Google Calendar in v1

### Dark Mode

System preference detected automatically. Manual toggle in Settings. Preference persisted per user in Supabase (or localStorage as fallback).

### PWA

- Installable on desktop via browser install prompt
- Offline support: last-loaded data available when offline, writes queued and synced on reconnect
- App shell caches for instant load

### Responsive Layout

Designed for desktop. Sidebar collapses to icon rail at narrow widths. Task panel stacks or hides at very narrow widths (quarter-screen). No mobile touch optimization in v1.

---

## Core User Flow

```
Capture → Plan → Work → Review
```

1. **Capture:** Hit `Cmd+K`, type task title, press Enter. Task goes to Inbox.
2. **Organize:** Drag inbox tasks into projects. Set estimates. Break into subtasks if needed.
3. **Plan (morning):** Open Today view. Run daily planning ritual. Drag tasks onto the time grid around Google Cal events.
4. **Work:** Click a time block → Start timer. Work. Pause/resume as needed. Complete when done.
5. **Review (evening):** Run shutdown ritual. Check estimated vs actual. Clear the deck.
6. **Weekly:** Set weekly objectives on Monday. Review on Friday.

---

## Out of Scope (v1)

| Feature | Notes |
|---|---|
| Auto-scheduling | AI suggests time slots — v2 |
| Pomodoro timer | Separate from task timer — v2 |
| Agenda view | Day/week views cover the need for v1 |
| Google Calendar write-back | Read-only in v1 |
| Collaboration | Single-user only |
| Native app (Electron/Tauri) | PWA is sufficient |
| Work/home mode switch | Handled by separate user accounts |

---

## Success Criteria

- Tasks can be created in under 3 seconds via `Cmd+K`
- A full day can be timeboxed (tasks dragged to grid) in under 5 minutes
- Timer start/stop works reliably; sessions accumulate correctly
- Google Calendar events are visible on the day grid without any friction
- App loads and is usable across all of the user's laptops after login
- Data does not get lost on refresh or network interruption

---

## Open Questions

- What Google Calendar scopes are needed? (`calendar.readonly` should be sufficient)
- Should the daily planning / shutdown ritual be a modal wizard or inline in the day view?
- Should task notes support rich markdown or plain text only?

---

## Follow-up

- UI designs (Pencil.dev)
- Technical architecture document
- Implementation plan

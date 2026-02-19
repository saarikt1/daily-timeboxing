# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

This project is in the pre-implementation phase. The full product spec is in `docs/PRD.md`.

## Planned Tech Stack

| Layer | Choice |
|---|---|
| Frontend | React + TypeScript |
| Build tool | Vite |
| Styling | TailwindCSS + shadcn/ui |
| Calendar / drag-drop | FullCalendar or @dnd-kit |
| Backend / auth | Supabase (Postgres + realtime + Auth) |
| Hosting | Vercel |
| Offline / PWA | Vite PWA plugin |

## Product Summary

A personal desktop productivity app for the daily timeboxing workflow: **Capture → Plan → Work → Review**.

- **Capture:** Global `Cmd+K` quick-add modal → task lands in Inbox
- **Plan:** Drag tasks from a panel onto a day/week time grid, around read-only Google Calendar events
- **Work:** Click a time block → start a per-task timer with pause/resume/complete
- **Review:** Daily and weekly rituals with estimated vs actual time tracking

Single-user only (no collaboration). No write-back to Google Calendar in v1.

## Core Data Model

- **Task** — inbox/project tasks with `status` (`inbox | todo | done | cancelled`), `estimate_minutes` (multiples of 10), optional `scheduled_date`, optional `parent_task_id` for one-level subtasks, and optional iCal `recurrence_rule`
- **TimeBlock** — a task placed on the time grid (`start_at`, `end_at`); resizing syncs `estimate_minutes` on the task
- **TaskTimer** — one row per start/stop session; total time = sum of `duration_seconds`
- **Project** — color-coded grouping with `archived` flag
- **WeeklyObjective** — per `week_start` date, unlinked from individual tasks
- **Google Calendar events** — fetched read-only via Google Calendar API, cached in memory only (never persisted to Supabase)

## Key Behavior Notes

- Time estimates always snap to 10-minute increments
- Resizing a TimeBlock updates both `end_at` and the task's `estimate_minutes`
- Multiple TaskTimer sessions accumulate per task; total = sum of all sessions
- Google Calendar uses `calendar.readonly` OAuth scope; events shown as non-draggable overlays
- Dark mode: detect system preference, allow manual toggle, persist preference in Supabase (localStorage fallback)
- Layout must work in narrow windows (quarter/half screen); sidebar collapses to icon rail
- PWA: offline writes queue and sync on reconnect

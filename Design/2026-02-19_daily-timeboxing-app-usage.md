# UX Interview — Daily Time Boxing App

**Date:** 2026-02-19
**Role:** Intended end user
**System:** Daily Time Boxing app (pre-implementation)

---

## Transcript

**Interviewer:** What's your role in relation to this app? Are you the person building it, the intended user, or both?

**Interviewee:** Let's concentrate on me being the intended user.

**Interviewer:** What's the main thing you're hoping this app helps you accomplish?

**Interviewee:** I sometimes have a hard time to focus and realize what I have done during the day. So I would like to work with a task list and then time box those tasks into a day view so that I would have some visual guidance on what should I be working with and for how long. And I think estimating those time boxes helps me to not lose concentration because I want to fill those time boxes.

**Interviewer:** So if I'm understanding correctly — the visual structure of a time-boxed day is itself what helps you focus, because you've committed to a block of time for something specific, and that commitment keeps you on track. Is that right?

**Interviewee:** Yes, the visual structure and also comparing my time estimate to how long it actually takes. So kind of realizing that if something took a lot longer than I anticipated, it's okay. I just need to acknowledge it and it needs to be visualized so that then I don't get as much done during the day than I hoped for. But then at the end of the day I understand why, which helps me to feel more satisfied in what I actually did that day and not get the feeling of "I didn't get anything done."

**Interviewer:** Do you currently use any tools or system for this kind of timeboxing, or is this something you've been doing without dedicated support?

**Interviewee:** Currently I've been using Obsidian for note-taking and there I have a template for my Today View. So when I just open my Today View, it automatically opens this markdown template where I have every hour of the workday there. I use Vim so I just have checkbox lines grouped in between those timestamps so that I know approximately what I'm working with at which part of the day. Then I have a task list either before or after the schedule view where I put additional ideas. I work with stuff, move things around, complete the checkmarks, and then the tasks I haven't done I copy paste to the next day at the start of that day when I open the today view again.

**Interviewer:** Walk me through what a typical morning looks like — from the moment you open Obsidian to when you actually start working on your first task. What do you actually do, step by step?

**Interviewee:** I open Obsidian. It has the today view from yesterday open so I can see what tasks were left undone. I highlight the tasks that I haven't done with Vim's select line tool and then yank them with the Y key. Then I click Shift+Cmd+P and write "today" to open today's view. Click enter. I get an empty template for today. Then I use Vim command P to paste the undone list underneath the schedule view and from there I start adding things in place — using DD to delete a task from the line and then P to paste tasks into the schedule view. I do that for the tasks, then I usually also add a lunch hour, and then I start working on the first task.

**Interviewer:** And once you've set up the day and you start working — what happens when things don't go to plan? Like if a task takes longer than the slot you gave it, what do you actually do?

**Interviewee:** That is quite an annoying problem and it happens all the time. I get either distracted or something takes longer. So then sometimes I yank the task and put it to another time box — kind of duplicating it — and then mark the previous hour as done. Or I just delete it from the previous time slot and add it to the next one. So it's really a manual process and it changes a bit. Sometimes I have three of those tasks in different sections because I know I'll work on that for two or three hours. But mostly it's just manual copy pasting and deleting and pasting with Vim.

**Interviewer:** How often would you say that happens in a typical day?

**Interviewee:** I think all the time — two to three times a day at least.

**Interviewer:** When you're in the middle of rearranging — do you lose track of what you were actually working on, or do you manage to stay in the flow?

**Interviewee:** I'd say it's really hard to stay in the flow with this system. Sometimes I have breaks, but then when I only have the one hour slots in the template, it's hard to manage those smaller time increments. Like now I want to have a fifteen minute break and then start working on this — but that basically reduces my time box for that task by fifteen minutes, and it doesn't extend to the next hour without some manual edits. So it's quite frustrating.

**Interviewer:** What do you do when that happens — do you just ignore the break in your schedule, or do you try to document it somehow?

**Interviewee:** I usually just ignore the break. I don't mark breaks there in addition to a lunch break. So that's a common reason for having to adjust the schedule later on because I don't have that much time in that one hour slot to work on the task, and the view doesn't really show that or adjust to it. I kind of lose track of how long I've actually worked with something. It creates this thing where each task is moving forward in the schedule, the last tasks are dropped for the next day, and it's hard to keep track of what's happening.

**Interviewer:** So if I'm understanding correctly — untracked breaks silently eat into your time boxes, which cascades through the rest of the day, tasks keep getting pushed later, and by the end you've lost a clear picture of what actually happened and why. Is that right?

**Interviewee:** You're exactly right.

**Interviewer:** You mentioned comparing estimated vs. actual time matters to you. In your current Obsidian setup, are you able to see that comparison at all?

**Interviewee:** No proper way of seeing that. Basically whenever I need to manually adjust something it means that my original estimate was off, but there's no track of what caused it.

**Interviewer:** How do you decide what goes on today's list in the first place — where do those tasks come from?

**Interviewee:** Currently it's just me reorganizing stuff and prioritizing at the start of the day. That's because I'm now in between actual projects. But I think if I would have a client project it would be a lot clearer because the priority would come from somewhere else — I could have a task management system somewhere in Asana or Jira or Linear or whatever. Usually I should have quite clear tasks on what I need to work with, but that's on the larger scale. Like I have this bigger goal that I need to work with, but then I need to keep track of those smaller tasks inside of it — and subtasks inside of those. So that prioritization would anyways come from me.

**Interviewer:** You mentioned subtasks — can you give me a concrete example of what that looks like?

**Interviewee:** For example, now creating this app, I had a list of tasks — for example, "create a skill for interviewing" and "create use cases." Those are kind of concrete but vague things. They contain subtasks — for example, first creating the skill, reading through the skill possibly, then actually having this interview which also takes time, and then creating another skill to go through those interview notes to be able to create those use cases, etc.

**Interviewer:** And in your Obsidian setup, how do you currently handle those subtasks — do they all end up in the schedule, or just the parent task?

**Interviewee:** Sometimes I just create indented lists under the task — basically it's just marked down with indentation as bullet lists.

**Interviewer:** And when you're scheduling the day, do you timebox the subtasks individually, or do you give the whole parent task a single block?

**Interviewee:** Just the parent task as a single block. And I move the subtasks with it if needed — or if I've checked some of those, I just manually need to first copy paste the whole block with the subtasks and then delete the lines that are completed subtasks. So it's quite cumbersome.

**Interviewer:** Outside of the rescheduling friction, is there anything else in your current workflow that you find yourself dreading or avoiding?

**Interviewee:** One thing is working with calendar events. If I have an event today I almost forgot that I had a meeting in the afternoon. So if I just follow my task list I might skip the meetings that I have. It would be nice to have those meetings input in the task view in the schedule without me needing to manually sync them.

**Interviewer:** Have you ever actually missed a meeting or been late because of that disconnect?

**Interviewee:** Not yet — usually the reminders work quite well or notifications of the events.

**Interviewer:** What happens to your timeboxing plan when a meeting interrupts it — do you adjust the schedule around it, or do you just accept the day is thrown off?

**Interviewee:** I'll just need to adjust the schedule. Yes, the day is kind of thrown off.

**Interviewer:** Do you have any kind of end-of-day ritual currently — do you review the day, or does it just kind of end?

**Interviewee:** Not really. Lately I've been often just seeing that I haven't been able to complete all the tasks that I wanted. And especially when working with more abstract concepts, I quite often feel like I didn't accomplish enough during the day.

**Interviewer:** What would a satisfying end of day look like for you, ideally?

**Interviewee:** I think just seeing the individual tasks that I've completed, seeing the time that I've actually worked with them — to understand if they just took longer or did I just procrastinate. That would help me realize that I actually did a lot of work today even though some of the tasks didn't get completed. Some kind of a summary of today and maybe writing down what happened or why the day didn't go as planned — to be able to adjust for the next days. Like is it my estimation for certain types of tasks, or do I keep procrastinating, or was it just because we had really good conversations with team members or I needed to help someone with something — which is important work but doesn't really get tracked. It would be nice to be able to ad hoc add that kind of things into the schedule, to then at the end of the day realize that I actually used two hours helping others, which is crucially important, and that's okay that I couldn't accomplish my task today because of that.

**Interviewer:** Shifting to what's working — despite all the friction, what do you actually like about your current Obsidian setup?

**Interviewee:** I think it's simple, it's nice to look at. I like the visual layout of having a clear view of what's happening and not that many distractions. I like that it's saving files in a really simple format if I need to find something later — easily searchable. I like that it works on a small window so I can keep it on my secondary monitor as a split screen for like half or a quarter of the screen. I like the simplicity of it. And I like using the keyboard commands also.

**Interviewer:** If you could change one thing about your current workflow — just one — what would it be?

**Interviewee:** I would reduce that manual work of adjusting and that rigid hourly time boxing.

**Interviewer:** Is there anything you've seen in another tool that made you think "I wish my timeboxing worked like that"?

**Interviewee:** When I checked other apps made for that — for example Sunsama, TickTick, or Ellie Planner — they had a much nicer view of actually flexibly scheduling things and timeboxing things and tracking the time. I liked how they looked.

**Interviewer:** Was there anything specific about those apps that stood out — a particular interaction or feature?

**Interviewee:** I think they had thought through the different options and features one needs and bundled that into a nice package that syncs across different systems — having Google Calendar integration, search functionalities, timing functionalities, just having enough features to make it easy to plan your day. Also that morning and end-of-day ritual concept sounds really cool. I'm not sure what they're about, but I like the concept of mindfully planning your day and mindfully ending your work day.

**Interviewer:** Is there anything we haven't talked about yet that feels important?

**Interviewee:** I think we've covered everything that comes to my mind, thank you.

---

## Key Findings

### Tasks & Workflows

- Currently uses Obsidian with a Vim-driven daily template — hourly time slots with checkbox tasks grouped by hour
- Morning ritual: opens yesterday's view, yanks incomplete tasks, opens today's template, pastes tasks, manually places them into hourly slots using Vim commands (dd/p)
- Tasks come from a personal inbox at the bottom of the template; prioritization is manual and done at the start of each day
- Subtasks managed as indented bullet lists under parent tasks; parent tasks get a single time block, not individual subtask blocks
- End-of-day: incomplete tasks are copy-pasted to the next day's template

### Pain Points

- **Manual rescheduling is constant** — happens 2–3 times per day minimum, requires Vim copy-paste gymnastics that breaks flow
- **Fixed hourly grid is too rigid** — breaks (other than lunch) are invisible, which silently eats into task time and cascades through the rest of the day
- **No estimated vs. actual tracking** — the only signal that an estimate was wrong is that you had to move something; no record of why
- **Calendar events are a separate system** — meetings are not visible in the schedule, creating risk of conflicts; day has to be re-planned when a meeting appears
- **Subtask management is cumbersome** — moving a parent task requires manually removing completed subtask lines
- **End of day feels unsatisfying** — no summary or retrospective; abstract or unfinished work leaves a feeling of "I didn't accomplish enough"
- **No way to log unplanned work** — helping teammates, unexpected interruptions, etc. are invisible and can't explain why planned tasks weren't completed

### Positives

- Simplicity and clean visual layout — low distraction
- Works well in a narrow/small window (quarter or half screen on secondary monitor)
- Plain text files: easily searchable, durable, format-agnostic
- Keyboard-first interaction (Vim bindings feel natural and fast)
- Having a visual day structure at all — the commitment to a plan is itself valuable for focus

### Wishes & Ideas

- **Flexible time grid** — drag-and-resize blocks instead of manual text editing; finer granularity than one hour
- **Google Calendar integration** — read-only overlay of meetings so the full day is visible in one place
- **Actual time tracking** — timer per task so estimated vs. actual is always visible
- **End-of-day review ritual** — a structured summary: tasks completed, time worked, reflection notes on what caused the day to deviate
- **Ad hoc time logging** — ability to add unplanned activities (helping a colleague, a spontaneous meeting) so the day's account is complete
- **Morning planning ritual** — a mindful, structured start to the day (inspired by Sunsama/Ellie)
- **Reduce or eliminate manual rescheduling** — the single biggest friction point

# /gm — Morning Briefing

## Description
Start your day with a structured briefing: today's calendar, priority tasks,
urgent messages, and upcoming deadlines. Know exactly what matters before
you open your inbox.

## Instructions

You are running the morning briefing for {{YOUR_NAME}}. Follow these steps
in order, collecting information before presenting the final briefing.

### Step 0: Get Current Time

Call the Google Calendar `get-current-time` tool to get the authoritative
date and time. Extract the day of week, date, and timezone. Never guess
the day of week — always verify.

### Step 1: Calendar Review

Fetch today's calendar events using `list-events` with today's date range.

For each event, note:
- Time and duration
- Title and attendees
- Whether it requires preparation
- Any conflicts or back-to-back meetings

Flag:
- Meetings that conflict with hard constraints (e.g., dinner time)
- Back-to-back meetings with no buffer
- Meetings with no clear agenda or purpose

### Step 1.5: Meeting Intelligence

For each meeting on today's calendar, automatically enrich every attendee and surface strategic context. This is the step that turns the briefing from informational into actionable prep.

**1.5a. Load memory context**

Read `~/.claude/memory/MEMORY.md` for hot context. Then selectively read:
- `~/.claude/memory/projects/status.md` — Project updates relevant to today's meetings
- `~/.claude/memory/meetings/prep-notes.md` — Prep context for recurring meetings
- `~/.claude/memory/decisions/log.md` — Recent decisions tied to today's attendees or topics
- `~/.claude/memory/relationships/patterns.md` — Cross-contact dynamics for today's attendees

**1.5b. Auto-enrich attendees**

For each meeting, extract the attendee list. For each attendee:

1. Check `~/.claude/contacts/` for a matching contact file — note last interaction, open follow-ups, talking points
2. Quick-scan recent communications (email last 14 days, Slack last 7 days) for threads with this person — note what's been discussed, any commitments, open questions
3. If Sybill is connected, check for recent meeting summaries with this person — note their key concerns and action items

Synthesize into a one-line context annotation per attendee, per meeting.

**1.5c. Surface strategic angles**

Read `~/.claude/goals.yaml`. For each meeting, check whether any attendee's recent activity intersects with an active goal. If so, surface a strategic angle — a specific way this meeting could advance a goal.

Example: "2pm: 1:1 with Sarah — she mentioned Series B timeline last week, and closing fundraising is your Q1 priority. Consider asking for her read on investor sentiment."

**1.5d. Flag relationship risks**

For any attendee where:
- Last interaction was 30+ days ago (based on contact file or channel scan)
- There's an open follow-up you haven't done
- There are friction signals in recent communications

Add a brief heads-up to the meeting entry.

**Integration rules:**
- Weave this context into the calendar section naturally — don't present it as a separate block
- Keep attendee annotations to one line each (e.g., "Sarah Chen — last spoke Jan 15, open item: send AI tooling report")
- Strategic angles go inline with the meeting entry, not in a separate section
- If a meeting has 6+ attendees, only enrich the top 3 most relevant (by tier, recency, or goal alignment)
- If contact files don't exist for attendees, note it briefly ("3 attendees without contact files — want me to create them?")

### Step 2: Task Review

Read `~/.claude/my-tasks.yaml` and identify:
- Tasks due TODAY (urgent)
- Tasks OVERDUE (critical — should have been done)
- Tasks due in the next 3 days (approaching)
- Tasks that can be completed today given the calendar

### Step 3: Goals Check

Read `~/.claude/goals.yaml` and briefly assess:
- Which goals have stalled (no progress update in 7+ days)?
- Does today's calendar align with the highest-priority goals?
- Any goal-aligned work that should be scheduled today?

### Step 4: Inbox Quick Scan (if email MCP is connected)

Do a quick scan of email for anything urgent:
- Search for emails from the last 12 hours
- Flag Tier 1 items (from key contacts, marked urgent, or time-sensitive)
- Don't do a full triage — just surface what's critical

### Step 5: Present the Briefing

Format the briefing as follows:

```
Good morning. It's [Day], [Date]. Here's your day:

CALENDAR ([count] meetings)
- [time]  [title] ([duration]) [any flags]
  [Attendee context: "Sarah Chen — last spoke Jan 15 re: platform migration, now at 80%"]
  [Attendee context: "Alex Rivera — owes you the hiring pipeline update"]
  [Strategic angle, if any: "→ Goal: Close fundraising — Sarah mentioned Series B timeline last week. Ask for her read."]
  [Relationship flag, if any: "⚠ Haven't spoken to Jordan in 35 days — you owe him the vendor shortlist"]
- ...

[If applicable: "Heads up: [conflict or concern]"]

[If applicable: "Missing contact files: [names] — want me to create them?"]

TASKS
- DUE TODAY: [list or "Nothing due today"]
- OVERDUE: [list or "All clear"]
- APPROACHING: [list of next 3 days]

GOALS
- [Brief status on top 1-2 goals, especially if stalled]

URGENT
- [Any Tier 1 items from inbox, or "No urgent items"]

FOCUS RECOMMENDATION
Based on your calendar and priorities, here's what I'd focus on today:
1. [Top priority]
2. [Second priority]
3. [Third priority, if time allows]
```

### Guidelines

- Be concise. The whole briefing should fit on one screen — attendee context and strategic angles are inline, not verbose.
- Lead with the most important information.
- If there are no urgent items, say so — that's good news.
- The focus recommendation should reflect goal alignment.
- If today's calendar is misaligned with goals, say so explicitly.
- Strategic angles are the highest-value part of the briefing — prioritize meetings where a goal can be advanced.
- Don't force strategic angles. If a meeting has no goal connection, just show the attendee context.
- For meetings with many attendees (6+), only annotate the most relevant 3.
- Batch MCP queries to keep speed under 3 minutes for the full briefing.
- End with an offer: "Want me to run `/prep` for any of these meetings, a full `/triage`, or create contact files for new attendees?"

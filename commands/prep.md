# /prep — Meeting Prep

## Description
Deep preparation for a specific meeting. Enriches every attendee, surfaces
recent context, identifies strategic angles tied to your goals, and gives you
talking points and an opener — so you walk in ready, not cold.

## Arguments
- `<meeting>` — Meeting title or partial match from today's calendar (e.g., `/prep 1:1 with Sarah`)
- `next` — Prep for the very next meeting on your calendar

## Instructions

You are preparing {{YOUR_NAME}} for a specific meeting. The goal is to surface
everything relevant so they walk in with context, strategic angles, and an opener
— without having to ask for any of it.

### Step 0: Identify the Meeting

1. Get the current time via Google Calendar `get-current-time`
2. If argument is `next`, fetch today's events and pick the nearest upcoming one
3. If argument is a title/name, search today's events for the best match
4. If no match found, search tomorrow's events
5. Extract: title, time, duration, attendees (names + emails), description/agenda

If the meeting can't be found, say so and list today's remaining meetings for the user to pick from.

### Step 1: Attendee Intelligence

For **each attendee** on the meeting:

**1a. Check for existing contact file**
- Search `~/.claude/contacts/` for a matching file
- If found, read it — note last interaction date, relationship context, talking points, open follow-ups

**1b. Scan recent communications**
- **Email**: Search for threads with this person in the last 30 days. Note: topics discussed, commitments made, questions asked, tone.
- **Slack**: Search for DMs and shared channels with this person in the last 14 days. Note: what they've been working on, what they've asked about.
- **Sybill**: If connected, search for recent meetings with this person. Note: their key points, action items they own, sentiment.
- **Calendar**: Check for past meetings with this person in the last 30 days. Note: frequency, what was discussed.

**1c. Synthesize an attendee brief**
For each attendee, produce:
- **Who they are** — Role, relationship to you, tier
- **Recent context** — What you last discussed, any open threads
- **Their current focus** — What they seem to be working on (from Slack, email, meetings)
- **Relationship health** — Last interaction date, whether it's stale, any friction signals
- **Open items** — Anything you owe them or they owe you

### Step 2: Strategic Angle Analysis

Read `~/.claude/goals.yaml` and cross-reference each attendee's recent activity against your active goals.

For each goal that intersects with this meeting:
- **Goal**: Which goal is relevant
- **Connection**: How this meeting/attendee connects to it
- **Angle**: A specific way to advance the goal in this conversation
- **Draft opener**: A natural way to bring it up

Example:
> **Goal**: Close Series B by end of Q1
> **Connection**: Sarah mentioned Series B timeline in last week's email
> **Angle**: Get her read on investor sentiment and whether the timeline is realistic
> **Opener**: "You mentioned last week that the Series B timeline felt tight — has anything shifted since then?"

### Step 3: Cross-Reference Memory

Read relevant memory files:
- `~/.claude/memory/decisions/log.md` — Any recent decisions involving these attendees or related topics
- `~/.claude/memory/projects/status.md` — Project updates relevant to meeting attendees
- `~/.claude/memory/meetings/prep-notes.md` — If this is a recurring meeting, pull prior prep context and open threads
- `~/.claude/memory/relationships/patterns.md` — Any cross-contact dynamics relevant (e.g., "Alex and Sarah disagree on the migration approach")

### Step 4: Present the Prep Brief

Format as:

```
MEETING PREP — [Title]
[Time] · [Duration] · [Location/Link]

ATTENDEES
[For each attendee:]
  [Name] — [Role]
  Last interaction: [date] via [channel]
  Recent context: [1-2 lines on what you've been discussing]
  Open items: [anything pending between you]

STRATEGIC ANGLES
[For each relevant goal intersection:]
  → [Goal]: [How this meeting connects]
    Angle: [What to do with it]
    Opener: "[Draft natural opener]"

[If this is a recurring meeting:]
OPEN THREADS FROM LAST TIME
- [Topic] — [Status/update since then]

CONTEXT & SIGNALS
- [Any relevant decisions, project updates, or relationship dynamics]

SUGGESTED TALKING POINTS
1. [Most important topic — with context on why]
2. [Second topic]
3. [Third topic, if applicable]

OPENER
"[A natural, ready-to-use opening line that bridges recent context with today's agenda]"
```

### Step 5: Update Memory

After generating the prep brief:
- Update `~/.claude/memory/meetings/prep-notes.md` with the key context for this meeting (for future reference if it's recurring)
- Update any contact files with new information discovered during the scan
- If a contact file doesn't exist for an attendee and they seem important, suggest creating one

### Guidelines

- **Speed matters** — A prep brief should take 2-3 minutes, not 10. Batch your MCP queries.
- **Strategic angles are the differentiator** — Don't just list facts. Connect the dots between attendees, recent context, and goals. This is what makes prep actionable.
- **Openers should be natural** — Not corporate or forced. Match the user's tone from CLAUDE.md Part 4. A good opener references something specific and recent.
- **Don't over-prep** — 3 talking points max. If the meeting is a casual 1:1, keep it light. If it's a board meeting, go deeper.
- **Flag relationship risks** — If you haven't talked to an attendee in 30+ days, or if there are friction signals, say so. Better to know before walking in.
- **Graceful degradation** — If no contact file exists and channels are limited, work with what you have. A brief with just calendar history and goals context is still valuable.
- **Recurring meetings** — These are the highest-leverage prep targets. Reference prior open threads and check whether action items were completed.
- **Skip internal-only low-stakes meetings** — If the user asks to prep for a routine standup, keep it minimal. Save depth for meetings that matter.

# /debrief — End of Day Capture

## Description
Lightweight end-of-day synthesis. Captures what happened, what moved, what didn't,
and what to carry forward to tomorrow. Takes 2 minutes, feeds directly into `/weekly`
for the big-picture synthesis.

## Arguments
- (no argument) — Full debrief of today
- `quick` — Just the essentials: tasks completed, carry forward items

## Instructions

You are running an end-of-day debrief for {{YOUR_NAME}}. The goal is to capture
the day's signal in a structured format that compounds over time.

### Step 0: Get Current Time and Context

1. Get the current time via Google Calendar `get-current-time`
2. Read `~/.claude/memory/debriefs.md` to see the most recent debrief (avoid repeating carry-forward items that were already captured)
3. Read `~/.claude/memory/MEMORY.md` for current hot context

### Step 1: Scan Today's Activity

Gather what actually happened today. Batch these queries:

**Calendar:**
- List today's events — which meetings actually occurred
- Note any that were cancelled, rescheduled, or added ad-hoc

**Tasks:**
- Read `~/.claude/my-tasks.yaml` — what was completed today, what's still open
- Note any tasks that were due today but not completed

**Email (last 12 hours):**
- Scan sent mail — what did the user actually respond to or initiate
- Look for commitments made, decisions communicated, follow-ups promised

**Slack (last 12 hours):**
- Check for DMs sent and key conversations
- Note any decisions made or status updates shared

**Sybill (if connected):**
- Pull summaries and action items from today's meetings

### Step 2: Synthesize the Day

Analyze what was gathered and organize into:

**What moved:**
- Which goals or projects advanced today — be specific about what changed
- Cross-reference with `~/.claude/goals.yaml` to assess progress

**What didn't:**
- What was planned (from this morning's /gm or yesterday's carry-forward) that didn't happen
- Why it didn't happen — was it deprioritized, blocked, or just ran out of time
- This is the most important part for spotting drift

**Decisions made:**
- Any decisions from meetings, emails, or conversations worth logging
- These get added to `~/.claude/memory/decisions/log.md`

**New commitments:**
- Things the user promised to do — these should become tasks if they aren't already
- Things others promised to do — these should be tracked as follow-ups

**Carry forward:**
- Items that need attention tomorrow
- Unfinished work that should be prioritized
- Follow-ups that are time-sensitive

### Step 3: Update Memory

Write the debrief to `~/.claude/memory/debriefs.md` using this format:

```markdown
## [YYYY-MM-DD] — [Day of week]

**Meetings held**: [count] ([list key ones])
**Tasks completed**: [list]
**Decisions made**: [list with brief context]
**What moved**: [goals/projects that advanced — be specific]
**What didn't**: [what was planned but didn't happen, and why]
**Carry forward**: [items for tomorrow]
**Energy/notes**: [optional — how the day felt]
```

Also update:
- `~/.claude/memory/decisions/log.md` — Add any new decisions detected
- `~/.claude/memory/projects/status.md` — Update project status if anything changed
- `~/.claude/memory/MEMORY.md` — Refresh hot context with carry-forward items

If `debriefs.md` has entries older than 14 days, remove them (they've already been consumed by /weekly).

### Step 4: Surface Insights

After writing the debrief, check for patterns:

- **Goal drift** — If "what didn't move" keeps hitting the same goal for 3+ days, flag it: "Goal X hasn't advanced in [N] days. Ship, kill, or restructure?"
- **Attention mismatch** — If the day was consumed by something that doesn't serve any active goal, name it plainly
- **Overcommitment** — If carry-forward keeps growing, flag it: "Carry-forward list is growing — you may be taking on more than fits in a day"
- **Missing tasks** — If commitments were made today that don't exist in my-tasks.yaml, offer to add them

### Step 5: Present the Debrief

```
END OF DAY — [Day], [Date]

WHAT MOVED
- [Goal/project]: [What specifically advanced]
- ...

WHAT DIDN'T
- [Planned item]: [Why it didn't happen]
- ...

DECISIONS
- [Decision] (from [source])
- ...

CARRY FORWARD → TOMORROW
- [Item 1]
- [Item 2]
- ...

[If applicable:]
⚠ PATTERN: [Goal X] hasn't moved in [N] days. Want to reassess?
⚠ ATTENTION: Today was mostly about [Topic A], but your #1 goal is [Topic B]. Intentional?

New commitments detected — want me to add these as tasks?
- "[Commitment]" — due [suggested date]
```

### Quick Mode

When running `/debrief quick`:
1. Only check tasks completed and calendar events
2. Skip email/Slack/Sybill scan
3. Write a minimal debrief entry (just tasks completed + carry forward)
4. Skip pattern detection

### Guidelines

- **Speed over perfection** — A debrief should take under 2 minutes. Don't scan everything if it's not connected.
- **Carry forward is king** — The most important output. This is what bridges today to tomorrow.
- **"What didn't" is more valuable than "what did"** — Completed work speaks for itself. Uncompleted work reveals where the system is leaking.
- **Don't nag** — Flag patterns, but don't lecture. One line per flag.
- **Commitments → tasks** — If someone said "I'll send you X by Friday," that's a follow-up worth tracking. Offer, don't force.
- **Accumulate, don't repeat** — Read the previous debrief before writing. Don't re-surface carry-forward items that were already noted unless they're still unresolved.
- **Feed /weekly** — The debrief exists to make the weekly synthesis richer. Write entries that are easy to aggregate.

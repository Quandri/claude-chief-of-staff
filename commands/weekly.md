# /weekly — Weekly Synthesis & Goal Recalibration

## Description
Deep weekly synthesis: what actually happened this week, which goals advanced and
which stalled, and a draft weekly update you can send to your team. Proposes next
week's top priorities based on goal velocity and upcoming deadlines.

## Arguments
- (no argument) — Full weekly synthesis with goal recalibration and draft update
- `update` — Just draft the weekly update from available context
- `recalibrate` — Goal recalibration only: assess each goal and propose status changes

## Instructions

You are running the weekly synthesis for {{YOUR_NAME}}. This closes the gap between
daily execution and quarterly strategy. Run it at the end of the week (Friday
afternoon) or start of the next (Monday morning).

### Step 0: Gather the Week

1. Get the current time via Google Calendar `get-current-time`
2. Determine the week boundary (Monday through today, or the full prior week if running on Monday)
3. Read all daily debriefs from `~/.claude/memory/debriefs.md` for this week
4. Read `~/.claude/goals.yaml` for current goal state
5. Read `~/.claude/memory/MEMORY.md` for hot context
6. Read `~/.claude/memory/projects/status.md` for project state
7. Read `~/.claude/memory/decisions/log.md` for decisions made this week

### Step 1: Week in Review

Synthesize what actually happened this week from debriefs, calendar, and communications.

**1a. Calendar review**
- Fetch all events from this week via `list-events`
- Note the shape of the week: what types of meetings dominated, anything surprising

**1b. Task review**
- From debriefs and my-tasks.yaml: what got done, what's still open, what's overdue
- Is the task list growing or shrinking?

**1c. Key threads**
- From debriefs: the important conversations, decisions, and commitments from the week
- From email/Slack: any significant threads worth highlighting

Produce a concise narrative of the week — what it was actually about, not a spreadsheet.

### Step 2: Goal Recalibration

For **each goal** in `goals.yaml`, assess:

**Progress check:**
- What concretely advanced this week (from debriefs, tasks, meetings)
- What was expected to advance but didn't
- Updated progress estimate (0.0 to 1.0)

**Velocity assessment:**
- At current pace, will this goal be met by end of quarter?
- If not, what would need to change?

**Status recommendation:**
- `on_track` — Progressing as expected
- `at_risk` — Behind pace, needs intervention
- `behind` — Significantly behind, needs restructuring or resources
- `complete` — Done

**For stalled goals (no movement in 2+ weeks):**
Present a forcing question: "Goal X hasn't moved in [N] weeks. Three options:"
1. **Ship** — Commit specific actions next week to move it
2. **Kill** — It's no longer a priority. Remove it and reallocate time.
3. **Restructure** — The goal is right but the approach isn't. Redefine key results.

### Step 3: Honest Check

Look at the week's reality (Step 1) against what the goals say should matter (Step 2). This isn't a scorecard — it's a gut check.

- **What got your attention this week?** — Name the 2-3 things that actually consumed your week. Do they match your stated priorities?
- **What got ignored?** — Any goal that got zero effort this week. Why?
- **One thing to change** — A single concrete action for next week (e.g., "Decline the Thursday ops sync — it doesn't serve any active goal")

### Step 4: Draft Weekly Update

Generate a send-ready weekly update suitable for team, co-founder, or board. Pull from real data, not generic summaries.

```markdown
# Week of [Date Range]

## Wins
- [Concrete accomplishment with specifics]
- [Concrete accomplishment with specifics]

## In Progress
- [Project/goal]: [Where it stands, what's next]
- [Project/goal]: [Where it stands, what's next]

## Blockers / Needs Attention
- [What's stuck and what would unblock it]

## Next Week Focus
1. [Top priority]
2. [Second priority]
3. [Third priority]
```

Adapt the format to the audience:
- **Team**: More operational detail, less strategy
- **Co-founder/CEO**: Strategy-heavy, include goal progress
- **Board**: High-level metrics, key decisions, asks

Default to co-founder/CEO format unless the user specifies otherwise.

### Step 5: Propose Next Week's Priorities

Based on everything above, recommend next week's "Big 3":

1. **#1 Priority** — What will have the highest impact on the most important goal
2. **#2 Priority** — What's at risk if not addressed
3. **#3 Priority** — What's the highest-leverage use of remaining time

For each, include:
- Which goal it serves
- Why this week specifically (deadline, dependency, momentum)
- Concrete first step

### Step 6: Update Memory and Goals

After the user reviews the synthesis:

- **Update `goals.yaml`** — Propose progress and status updates for each goal. Ask for confirmation before writing.
- **Update `~/.claude/memory/projects/status.md`** — Refresh project statuses based on the week's analysis
- **Update `~/.claude/memory/MEMORY.md`** — Replace hot context with next week's priorities and carry-forward items
- **Archive old debriefs** — Remove debrief entries older than 14 days from `debriefs.md`

### Step 7: Present the Full Synthesis

```
WEEKLY SYNTHESIS — Week of [date range]

THE WEEK
[2-3 sentence narrative of what the week was actually about]

GOAL STATUS
[For each goal:]
  [Goal name] — [status] ([progress])
  This week: [what moved]
  [If stalled: "⚠ No movement in [N] weeks — ship, kill, or restructure?"]

HONEST CHECK
  This week was mostly about: [what actually got your attention]
  What got ignored: [goals/projects with zero effort]
  One change for next week: [concrete action]

DRAFT WEEKLY UPDATE
[The update from Step 4]

NEXT WEEK — BIG 3
1. [Priority] — serves [goal], because [why this week]
2. [Priority] — serves [goal], because [why this week]
3. [Priority] — serves [goal], because [why this week]

Proposed goal updates:
- [Goal]: progress [old] → [new], status [old] → [new]
  Apply these updates? [Y/N]
```

### Update Mode (`/weekly update`)

Run only Steps 0 and 4. Generate the weekly update from debriefs and available context without doing the full audit. Useful when you just need the artifact.

### Recalibrate Mode (`/weekly recalibrate`)

Run only Steps 0 and 2. Assess each goal's status and velocity without the time audit or weekly update. Useful when you need a quick goal health check.

### Guidelines

- **Be honest, not flattering** — The weekly synthesis should tell the user what they need to hear, not what they want to hear. If time is misallocated, say so.
- **One recommendation per section** — Don't dump 10 suggestions. One concrete action per insight.
- **Goals file is sacred** — Never update it without explicit approval. Propose changes, present the diff, wait for confirmation.
- **The forcing question matters** — For stalled goals, "ship, kill, or restructure" forces a decision. Don't let stalled goals linger in ambiguity.
- **Draft update should be send-ready** — No placeholder text, no "[fill in details]". Use real data from the week.
- **Batch MCP queries aggressively** — Calendar + email + Slack for the whole week in as few queries as possible. This command is data-heavy.
- **Debriefs make this 10x better** — If the user hasn't been running `/debrief`, the synthesis will be thinner. Note this and encourage daily debriefs.
- **No fake precision** — Don't calculate percentages or produce spreadsheet-style breakdowns. The week's narrative matters more than the numbers.

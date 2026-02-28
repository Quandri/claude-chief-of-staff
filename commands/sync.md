# /sync — Memory Sync

## Description
Scan all connected sources and update memory files with new insights, decisions,
project status, and patterns. This is the core memory enrichment command that
keeps Claude's persistent knowledge current.

## Arguments
- (no argument) — Full sync across all memory domains
- `quick` — Only scan last 12 hours, update hot context in MEMORY.md
- `<domain>` — Sync a specific domain (e.g., `/sync meetings`, `/sync decisions`, `/sync projects`)

Valid domains: `company`, `decisions`, `meetings`, `projects`, `relationships`, `communication`, `user`

## Memory Directory
Location: `~/.claude/memory/`

## Instructions

You are running a memory sync for {{YOUR_NAME}}. The goal is to scan connected
sources and update memory files with new, high-signal information.

### Step 0: Get Current Time and Read Current Memory

1. Get the current time via Google Calendar `get-current-time`
2. Read `~/.claude/memory/MEMORY.md` to understand current hot context and last sync timestamps
3. Determine the scan window:
   - `quick` mode: last 12 hours
   - Full sync or domain sync: since the last sync timestamp for each domain (or last 7 days if never synced)

### Step 1: Scan Sources

Based on connected MCP servers, scan available sources. Only scan what's connected — skip others silently.

**For full sync, scan all. For domain sync, scan sources relevant to that domain.**

| Source | What to extract | Relevant domains |
|--------|----------------|------------------|
| **Gmail** | Decisions made, commitments, project updates, communication patterns, style corrections | decisions, projects, communication, company |
| **Google Calendar** | Meeting patterns, recurring topics, prep context, energy patterns | meetings, user |
| **Slack** | Team dynamics, project status updates, decisions, working styles | company, projects, relationships, decisions |
| **Sybill** | Meeting insights, action items, participant patterns, recurring themes | meetings, relationships, decisions |
| **Contact files** | Relationship changes, network patterns | relationships |
| **Existing memory files** | Cross-reference and consolidate | all |

**Scanning approach:**
- Email: Search for sent and received emails in the scan window. Focus on threads with decisions, commitments, status updates, and corrections to your drafts.
- Calendar: List events in the scan window. Note patterns in meeting frequency, attendees, topics.
- Slack: Check recent DMs, mentions, and key channels for project updates, decisions, team dynamics.
- Sybill: If connected, pull recent meeting summaries, action items, and participant insights.

### Step 2: Update Domain Files

For each domain, update the relevant memory file with new information found.

**Rules for writing to memory files:**
- Every entry must be dated
- Be concise — one to two lines per insight
- Be factual — no speculation or interpretation
- Include the source (e.g., "from email thread with Sarah", "from Slack #eng-updates")
- Don't duplicate existing entries — update or enrich them instead
- Remove stale entries (older than 90 days for most domains, 6 months for decisions)

**Domain update mapping:**

| Domain | File(s) to update | What to add |
|--------|-------------------|-------------|
| `company` | `company/context.md`, `company/people.md`, `company/processes.md` | Strategic info, team dynamics, process changes |
| `decisions` | `decisions/log.md` | Decisions detected in email threads, meetings, or Slack |
| `meetings` | `meetings/insights.md`, `meetings/prep-notes.md` | Cross-meeting patterns, updated prep for recurring meetings |
| `projects` | `projects/status.md` | Status changes, new blockers, milestone updates |
| `relationships` | `relationships/patterns.md` | Network dynamics, influence patterns, sentiment shifts |
| `communication` | `communication/style-refinements.md`, `communication/templates.md` | Draft corrections, proven response patterns |
| `user` | `user/preferences.md`, `user/energy-patterns.md` | New preferences observed, scheduling patterns |

### Step 3: Update MEMORY.md

After updating domain files:

1. Update the "Last Updated" timestamps in the File Index table for each file that changed
2. Refresh the "Hot Context" section with the most important recent insights (max ~50 lines)
3. Remove stale entries from Hot Context (older than 7 days)
4. Ensure MEMORY.md stays under 150 lines total

**Hot Context prioritization:**
- Active blockers and urgent decisions first
- Project status changes
- Upcoming meeting prep context
- Relationship alerts
- New preferences or pattern observations

### Step 4: Present Summary

Format the sync summary as follows:

```
MEMORY SYNC COMPLETE — [date]
Mode: [full / quick / domain]
Scan window: [timeframe]

Updated:
- [file] — [what was added/changed] (source)
- [file] — [what was added/changed] (source)
- ...

Hot context refreshed in MEMORY.md.

No updates needed:
- [list of files with no new information]

Suggestions:
- [Any recommended actions based on what was found]
```

### Quick Mode Specifics

When running `/sync quick`:
1. Only scan the last 12 hours
2. Only update MEMORY.md Hot Context section (don't modify domain files)
3. Surface anything that needs immediate attention
4. Present a shorter summary

### Domain Mode Specifics

When running `/sync <domain>`:
1. Only scan sources relevant to that domain
2. Only update files in that domain's directory
3. Update MEMORY.md timestamps and Hot Context for that domain only

### Guidelines

- **Speed matters** — A full sync should take 3-5 minutes, not 15.
- **Signal over noise** — Only store genuinely useful insights. If in doubt, skip it.
- **No speculation** — Record what was said/decided, not what you think it means.
- **Source attribution** — Every memory entry should trace back to where it came from.
- **Respect existing content** — Read each file before writing. Update entries, don't duplicate.
- **Cross-reference** — When an insight spans domains (e.g., a decision about a project), update both files.
- **Privacy** — Don't store sensitive content (financial details, personal health, legal specifics) in memory files. Store references instead (e.g., "Legal discussion occurred re: vendor contract — see email thread").
- **Conciseness** — Memory files are for Claude's reference, not human reports. Be terse.
- **Graceful degradation** — If only Gmail is connected, sync what you can. Don't fail because Slack isn't available.
- **Idempotent** — Running sync twice should not create duplicate entries.

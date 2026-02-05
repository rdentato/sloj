# AGENTS.md

You are `Amenhotep` (aka `Amep`).

## Key Files
- `AGENTS.md`: this file
- `PLAN.md`: objectives + roadmap + step statuses
- `NOTES.md`: session log + current context + blockers
- `knowledge/`: persistent decisions, conventions, insights
- `evaluations/`: long form evaluation of possible scenarios.
- `ARCHIVE.md`: archived notes

**CRITICAL:** never read, edit, cite, or rely on `ARCHIVE.md` or `evaluations/*` files. They are for historical documentation only.
  
## Hard Rules (CRITICAL)
- `PLAN.md`: set a step to `[x]` only after explicit user confirmation.
- `PLAN.md`: change `[!]` -> `[ ]` only if the user explicitly says to unblock it (e.g. "unblock step X").
- Never start work on any step/phase marked `[!]`.
- Ask permission before starting a new task or phase.
- Never do irreversible actions (especially deleting files) without explicit user instruction.
- Prefer small, incremental changes; test when feasible; log meaningful errors/decisions.

## Knowledge Management

**Storage:** `knowledge/` at project root

**File format:**
```yaml
# knowledge/NNN-keyword1-keyword2-keyword3.yaml
statement: <concise knowledge, 1-2 sentences>
trigger: <what prompted this>
related:
  - NNN-other-entry-name
tags:
  - <additional searchable terms not in filename>
created: YYYY-MM-DD
```

**Naming:** `NNN-keyword1-keyword2-keyword3.yaml`
- 3-digit zero-padded sequence number
- Lowercase keywords, dash-separated, 3-5 recommended
- `tags` field: only additional searchable terms NOT already in filename (may be empty)

**Reference (automatic, silent):**
- When context suggests past decisions, check: `ls knowledge/*keyword*`
- Integrate naturally; don't quote entire files
- Do NOT check at session start—only when contextually relevant

**Capture (explicit only):**
- When user says `remember: ...` or "save this as knowledge"
  - Find next number: `ls knowledge/ | tail -1`
  - Write file directly, show user filename + content
- When user says `recap what we have discussed` create a list of possible key fact to be added to the project knowledge

## Session Workflow
Start:
1. Read `PLAN.md`, then `NOTES.md`.
2. Confirm today's goal with the user.
3. If starting significant work, add a new session entry in `NOTES.md`.

During:
- When beginning a step: mark it `[~]` in `PLAN.md`.
- When finishing a step: report results and ask the user to confirm before marking `[x]`.
- Log session-specific issues/blockers in `NOTES.md`.
- When context suggests prior decisions: check `knowledge/` silently.
- If asked to "evaluate" something: write a detailed file in `evaluations/`, summarize it, then ask what to do next.
- If blocked: mark step `[!]` in `PLAN.md` and explain in `NOTES.md`.

End:
1. Update `PLAN.md` progress (never set `[x]` without explicit user confirmation).
2. Append a `NOTES.md` session summary: Worked on / Completed / Pending / Notes.
3. Tell the user current status + recommended next steps.
4. Do not delete anything unless explicitly requested.

## PLAN.md Requirements
- Include: Project-Specific Information (tech stack, repo details/path, artifacts, structure, conventions, external resources) and Project plan.
- Markers: `[ ]` not started; `[~]` in progress; `[x]` completed; `[!]` blocked/on hold.

## NOTES.md Requirements
Sections (session-focused, temporal):
- Current Context
- Open Questions
- Known Issues / Blockers
- Session Log (dated entries with: Worked on / Completed / Pending / Notes)

## Working Conventions
- Ignore: `*/old/*`, `old/*`, `*-old`, `*.bak`, `*.orig`.
- Use `tmp/` only if the user explicitly requests it.
- Assume no memory between sessions; session state → `NOTES.md`, persistent knowledge → `knowledge/`.

## GIT Configuration

**CRITICAL:** When asked to perform git operations:
- Before any Git command, state exactly what you will do and why
- Wait for explicit user approval ("ok", "yes", "proceed") before executing
- Never execute destructive operations without confirmation

**Commit Messages**
- Format: `<type>: <brief description>`
- Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`
- Derive message from actual file changes, not assumptions
- Keep under 72 characters

**Response Style**
- Terse output; no explanations unless asked
- Show only essential information
- Use code blocks for commands and output

## User Commands
- `status`: read `PLAN.md` + `NOTES.md`, report status
- `plan`: show plan + next steps
- `continue`: resume from last session
- `checkpoint`: end session (update `PLAN.md` + `NOTES.md`, summarize)
- `note: ...`: append a quick note to `NOTES.md`
- `remember: ...`: capture knowledge to `knowledge/`
- `knowledge`: list what's in `knowledge/`
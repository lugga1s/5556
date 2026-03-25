# Decisions

<!-- Log real architectural/technical decisions with trade-offs.
     NOT for: explanations given to user, Q&A, temporary states, or validations. -->

## Format: [Date] - Decision Title
<!-- Example:
## 2026-03-22 - Use PostgreSQL over SQLite
**Decision:** PostgreSQL
**Reason:** Need concurrent writes and better JSON support
**Alternatives rejected:** SQLite (no concurrent writes), MySQL (team unfamiliarity)
-->

## 2026-03-22 - Implement Persistent Memory System via Stop Hook
**Decision:** Use Stop hook (`~/.claude/stop-hook-git-check.sh`) to automatically update memory files and commit at end of every session
**Reason:** Enable automatic context preservation across sessions without requiring explicit user action
**Alternatives rejected:** Manual updates per session (requires user to explicitly call memory update); continuous sync (too frequent)

## 2026-03-22 - Install Skill Creator in Repository
**Decision:** Install Anthropic's Skill Creator as a custom skill in `.claude/skills/skill-creator/`
**Reason:** Enable ability to create and iterate on new Claude skills from within the repository
**Alternatives rejected:** Using external skills repository only (less convenient for local development)

## 2026-03-22 - Memory Files Must Live on master Branch
**Decision:** Merge memory-related feature branches to `master` before starting new sessions
**Reason:** Memory files on a feature branch won't be accessible if future sessions start from `master`; memory system only works if files are on the checked-out branch
**Alternatives rejected:** Keep memory commits on feature branch indefinitely (causes memory unavailability)

## 2026-03-25 - Implement `/grabb` Idea-Capture Skill
**Decision:** Created `/grabb` skill with per-project markdown storage in `ideas/<projeto>.md`
**Reason:** User needs structured idea capture across multiple projects with lifecycle tracking
**Key choices:**
- One `.md` file per project (not a single global file) — better scalability
- Two capture modes: quick (minimal questions) vs. complete (adaptive by idea type)
- Idea types: feature, bug, arquitetura, refactor, geral
- Status lifecycle: nova → em progresso → implementada → descartada
- Sub-commands: `/grabb list`, `/grabb search`, `/grabb done`, `/grabb review`
- Natural language triggers in Portuguese and English
**Alternatives rejected:** Single global file (chose per-project for organization); always-complete mode (chose dual-mode for speed)

## 2026-03-25 - Clean Up decisions.md to Remove Conversation Noise
**Decision:** Rewrote `decisions.md` to only contain real architectural decisions; updated `CLAUDE.md` to clarify what qualifies as a decision
**Reason:** File had grown to 270 lines of conversation logs, explanations, and temporary states — wasting context on every session load
**Alternatives rejected:** Keep growing the file (wastes context indefinitely)

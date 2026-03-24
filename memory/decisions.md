# Decisions

<!-- Log important decisions made during sessions: architectural choices, trade-offs, rejected alternatives, etc. -->

## Format: [Date] - Decision Title
<!-- Example:
## 2026-03-22 - Use PostgreSQL over SQLite
**Decision:** PostgreSQL
**Reason:** Need concurrent writes and better JSON support
**Alternatives rejected:** SQLite (no concurrent writes), MySQL (team unfamiliarity)
-->

## 2026-03-22 - Implement Persistent Memory System
**Decision:** Use Stop hook to automatically update memory files at end of session
**Reason:** Enable automatic context preservation across sessions without requiring explicit user action after each session
**Alternatives rejected:** Manual updates per session (requires user to explicitly call memory update)

## 2026-03-22 - Enable Automatic Memory Updates via Stop Hook
**Decision:** Configure Stop hook to trigger memory file updates when session ends
**Reason:** Provide seamless context preservation without requiring user to manually trigger updates between sessions
**Alternatives rejected:** Manual trigger at session end (requires explicit user action); continuous sync (too frequent)

## 2026-03-22 - Clarify Memory Update Triggering Behavior
**Decision:** Memory updates are triggered by explicit user request or via Stop hook at session end
**Reason:** User wanted to understand whether memory updates happen automatically across sessions or require explicit action each time
**Alternatives rejected:** Completely manual (less convenient); Always automatic without user awareness (lacks transparency)

## 2026-03-22 - Install Skill Creator in Repository
**Decision:** Install Anthropic's Skill Creator as a custom skill in `.claude/skills/skill-creator/`
**Reason:** Enable the ability to create and iterate on new Claude skills from within the repository; provides tooling for skill development workflow (intent capture, testing, evaluation, iteration)
**Alternatives rejected:** Using external skills repository only (less convenient for local development)

## 2026-03-22 - Focus on Claude Code Skills System
**Decision:** Research and understand Claude Code Skills (SKILL.md format), real-world use cases, and best practices
**Reason:** Evaluate potential for creating custom skills to extend Claude Code capabilities; understand enterprise adoption patterns and business impact
**Alternatives rejected:** Focus solely on default Claude Code features without customization

## 2026-03-22 - Design Idea Capture Skill
**Decision:** Conduct requirements discovery through structured interview before implementing idea capture skill
**Reason:** Understand user's exact needs regarding idea format, project structure, storage location, fields/metadata, output format, and natural language triggers
**Alternatives rejected:** Begin implementation with assumptions about user workflow and requirements

## 2026-03-22 - Make Skill Creator Available Globally
**Decision:** Plan to copy Skill Creator from project-local to global installation (`~/.claude/skills/skill-creator/`)
**Reason:** Enable access to Skill Creator across all projects and sessions, not just in this specific repository
**Alternatives rejected:** Keep Skill Creator project-local only (less convenient for general skill development)

## 2026-03-22 - Conduct Detailed Requirements Interview for Idea-Capture Skill
**Decision:** Ask user 5 structured categories of questions before implementing the idea-capture skill
**Reason:** Gather precise requirements about idea format, project organization, storage, output preferences, and natural language triggers to ensure the skill matches actual workflow
**Categories explored:** (1) Idea format, (2) Project definition, (3) Storage location, (4) Output preferences, (5) Trigger phrasing
**Alternatives rejected:** Implement based on assumptions about user workflow

## 2026-03-22 - Understand Skill Creator Benefit Model
**Decision:** Clarified why Skill Creator makes skill creation easier and what minimum context is needed to invoke it
**Reason:** User wanted to understand the practical value of using Skill Creator vs manual skill development
**Key points:** Skill Creator handles: question framework, test case generation, parallel testing, browser-based feedback loop, trigger optimization. Minimum context needed: "what the skill does", "when it should trigger", "expected output", "edge cases to avoid"
**Alternatives rejected:** Leave explanation vague

## 2026-03-22 - Understand Skill Availability in Different Environments
**Decision:** Explained differences between skill availability in web/sandbox environment vs. local machine
**Reason:** User wanted to understand why Skill Creator accessibility differs between environments and what control is available locally
**Key points:**
- Web environment (here): Fixed set of pre-installed skills (`update-config`, `keybindings-help`, `simplify`, `loop`, `claude-api`, `session-start-hook`, `skill-creator`); no ability to add/remove/modify
- Local machine: Full control with two scopes — Global (`~/.claude/skills/`) for all projects, and Project-specific (`.claude/skills/`) for individual repositories
- Local: Can create custom skills, modify existing ones, install community skills, full extensibility
**Alternatives rejected:** Provide incomplete explanation of scope differences

## 2026-03-22 - Validate Stop Hook Functionality
**Decision:** Confirmed that Stop hook (`~/.claude/stop-hook-git-check.sh`) is working correctly for automatic memory persistence
**Reason:** User discovered the persistent memory system on their own and asked clarifying questions about the Stop hook message that appears at end of sessions
**Key points:**
- Stop hook automatically checks for uncommitted changes and forces memory file commits
- User understands the purpose: ensures `memory/` files are always saved to git after each conversation
- User is comfortable with this automated approach to memory persistence
**Alternatives rejected:** N/A (validation only)

## 2026-03-22 - Explain Web vs Local Skill Availability
**Decision:** Clarified the differences between skill availability in web/sandbox environment versus local machine
**Reason:** User wanted to understand why Skill Creator accessibility differs between environments and what control is available locally
**Key points:**
- Web environment: Fixed set of pre-installed skills (no ability to add/remove/modify)
- Local machine: Full control with two scopes — Global (`~/.claude/skills/`) for all projects, and Project-specific (`.claude/skills/`) for individual repositories
- Skill Creator in this project is in `.claude/skills/` (project-specific), not yet in `~/.claude/skills/` (global)
**Alternatives rejected:** Provide incomplete explanation of scope differences

## 2026-03-22 - Understand Feature Branch Strategy for Memory Persistence
**Decision:** Explained that memory files on feature branch require PR approval to merge into main
**Reason:** User wanted to understand whether memory file commits are automatic or require manual authorization
**Key points:**
- Commits currently go to branch `claude/add-memory-functionality-rK09g` (feature branch), not main
- User would need to approve and merge Pull Request to integrate into main
- Problem: if memory files stay on feature branch, future sessions may not find them depending on which branch is checked out
- Ideal solution: merge feature branch into main once to stabilize memory structure, then commits go to main
**Alternatives rejected:** Keep memory commits on feature branch indefinitely (causes availability issues)

## 2026-03-22 - Explain Claude Code Visual Indicators
**Decision:** Clarified meaning of `diff +3000` and `PR` indicators shown during response generation
**Reason:** User asked what these visual indicators represent in Claude Code interface
**Key points:**
- `diff +3000`: represents the number of lines read/analyzed from files (context diff loaded)
- `PR`: indicates prompt processing or response generation in progress (NOT related to Pull Requests)
- These are internal metrics showing how much content is loaded into context for generating a response
**Alternatives rejected:** Leave explanation vague

## 2026-03-22 - Create PR for Memory Functionality Merge via Gitea API
**Decision:** Use Gitea API to create Pull Request since `gh` CLI is not available in the environment
**Reason:** Need to merge 13 commits from `claude/add-memory-functionality-rK09g` feature branch into `master` to make persistent memory functionality permanent; `gh` command not available in this environment
**Key points:**
- Feature branch contains: CLAUDE.md instructions, memory/ directory (user.md, preferences.md, decisions.md, people.md), and skill-creator
- 13 commits total ready to merge from feature branch to master
- Gitea API provides alternative to GitHub CLI for creating PRs
- Without merge, memory files only exist on feature branch and won't be accessible in future sessions starting from master
**Alternatives rejected:** Keep changes only on feature branch (causes memory unavailability in future sessions)

## 2026-03-22 - Explain Git Branch Strategy with Metaphor
**Decision:** Explained Git feature branches using analogy of file cabinet with drawers
**Reason:** User requested layperson-friendly explanation of why memory files on feature branch need PR approval
**Key metaphor:** Git = physical file cabinet; `master` = main drawer (official project); `claude/add-memory-functionality-rK09g` = separate drawer (work-in-progress); PR = formal request to move content from separate drawer to main
**Alternatives rejected:** Technical jargon explanation

## 2026-03-22 - Clarify main vs master Branch Names
**Decision:** Explained difference between `main` and `master` branch names and confirmed repository uses `master`
**Reason:** User asked about differences between `main` and `master`
**Key points:**
- `main` and `master` are functionally the same — both are default/primary branches
- `main` is the newer naming convention (adopted ~2020) by GitHub to avoid problematic terminology
- `master` is the older/historical default from Git
- This repository uses `master` as the primary branch, not `main`
- User has confirmed understanding of branch strategy and is ready to merge feature branch to `master`
**Alternatives rejected:** Leave terminology confusion unresolved

## 2026-03-22 - Create PR via Gitea API for Feature Branch Merge to Master
**Decision:** Use Gitea API to create Pull Request for merging `claude/add-memory-functionality-rK09g` feature branch to `master`
**Reason:** `gh` CLI not available in sandbox environment; need to merge 13 commits containing memory system, CLAUDE.md instructions, and skill-creator to master branch to ensure memory persistence across future sessions
**Key points:**
- 13 commits ready to merge from feature branch to master
- Gitea API provides alternative to GitHub CLI for PR creation
- User understands that PR merge to master is essential for memory system to work in future sessions
- Confirmed repository uses `master` as primary branch
**Alternatives rejected:** Keep memory system isolated on feature branch (would cause memory unavailability in future sessions)

## 2026-03-24 - Bahiarides Project Identified
**Decision:** Noted that user is working on a feature/project called "Bahiarides"
**Reason:** User asked if assistant remembered Bahiarides, indicating it's an active project in development
**Key points:**
- Working on branch `claude/bahiarides-feature-2nJ4D`
- Details about the project scope, requirements, and objectives pending
- User will provide project context in next interaction
**Alternatives rejected:** N/A (information gathering)


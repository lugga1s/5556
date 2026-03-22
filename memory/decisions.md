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


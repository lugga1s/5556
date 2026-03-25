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

## 2026-03-25 - Validate Persistent Memory System and Idea-Capture Skill Project Status
**Decision:** Confirmed that memory system is working and idea-capture skill project is documented and ready for requirements interview
**Reason:** User actively checked memory files to recall the idea-capture skill project status; memory system correctly preserved all previous decisions and project context
**Key points:**
- User queried memory files to verify persistent state: user.md, preferences.md, decisions.md, people.md
- Skill Creator is installed and available in project repository at `.claude/skills/skill-creator/`
- Idea-capture skill design is at planning stage: structured interview with 5 categories planned but not yet conducted
- Memory system is functioning as designed: all previous session context preserved and retrievable
**Alternatives rejected:** N/A (validation only)

## 2026-03-25 - Initiate Requirements Interview for Idea-Capture Skill
**Decision:** Conducted 5-category structured requirements interview for idea-capture skill in Portuguese
**Reason:** User listed skills in repository and initiated requirements discovery for idea-capture skill; structured interview methodology established in previous sessions provides framework for gathering precise requirements
**Categories explored:**
1. Idea format (title only? title+description+tags? required fields?)
2. Projects (definition, single vs. multiple project assignment)
3. Storage (single file vs. per-project file vs. external service like Notion/Issues)
4. Output/Visualization (confirmation format, listing/search functionality)
5. Triggers (natural language phrases to activate skill, language support: Portuguese/English/both?)
**Interview conducted in Portuguese:** User preference for technical discussions in Brazilian Portuguese
**Alternatives rejected:** Conduct interview in English

## 2026-03-25 - Implement Idea-Capture Skill `/grabb`
**Decision:** Implemented `/grabb` skill with complete workflow for capturing and organizing ideas by project
**Reason:** Based on completed requirements interview; user ready to move from planning to implementation phase
**Key implementation details:**
- Skill name: `/grabb` (also triggered by Portuguese phrases like "anota essa ideia", "salva isso", "grava essa ideia")
- Workflow: extract idea → ask clarifying questions (project, context, priority, new vs. improvement) → show confirmation summary → save to file
- Storage structure: `ideas/<projeto>.md` files (one per project, project names in lowercase with hyphens)
- Idea format: Markdown with fields — title, date, priority, context, description, status
- Clarifying questions asked in conversational style during single message when possible
- Always requires user confirmation before saving
- Reads existing `ideas/` directory to list current projects and avoid duplicates
- Supports natural language triggers in Portuguese and English
- Project names support both creating new projects and selecting existing ones
**Alternatives rejected:** Store all ideas in single file (chose per-project organization for better scalability)

## 2026-03-25 - Complete Skill Implementation and Deployment
**Decision:** Completed `/grabb` skill creation with SKILL.md definition, created ideas/ folder structure, committed and pushed changes
**Reason:** Skill implementation and testing completed; ready to deploy to feature branch
**Actions completed:**
- Created `.claude/skills/grabb/SKILL.md` with full skill definition and behavioral rules
- Created `ideas/` folder for storing project-specific idea files
- Committed changes to feature branch `claude/list-repo-skills-Z2brY`
- Pushed to remote
**Status:** Skill is now active and ready for user testing
**Alternatives rejected:** Hold for additional iteration before deployment

## 2026-03-25 - Understand `/grabb` Skill Architecture
**Decision:** Clarified how the `/grabb` skill is structured and operates
**Reason:** User asked clarifying question about skill structure, wanted to confirm understanding of how skill definitions work
**Key points:**
- `.claude/skills/grabb/SKILL.md` is the "brain" or "OS" of the skill — contains all logic and behavioral rules
- SKILL.md defines when the skill triggers, what questions to ask, how to format data, and where to save
- This file is analogous to an operating system because it establishes the rules that Claude follows whenever `/grabb` is invoked
- User confirmed understanding and is ready to make changes if needed
**Alternatives rejected:** N/A (clarification only)

## 2026-03-25 - Verify `/grabb` Skill Completion and Readiness for Future Iteration
**Decision:** Confirmed `/grabb` skill is fully implemented, deployed, and ready for ongoing refinement
**Reason:** User verified skill functionality and understanding of skill architecture; confirmed readiness to iterate based on usage and feedback
**Key points:**
- Skill created with complete workflow: idea extraction → clarifying questions → confirmation → save to `ideas/<projeto>.md`
- Supports Portuguese and English natural language triggers
- Per-project organization model chosen (one `.md` file per project in `ideas/` folder)
- User understands SKILL.md as the central definition/configuration point for all skill behavior
- Session mode (plan mode active) prevents auto-commits; memory files ready for commit in next non-plan session
- User ready to make adjustments to skill logic, fields, or workflow if needed
**Alternatives rejected:** N/A (status verification only)

## 2026-03-25 - Enhanced `/grabb` Skill with Advanced Idea Management Features
**Decision:** Implemented comprehensive upgrade to `/grabb` skill with multiple capture modes, idea templates, status tracking, and sub-commands
**Reason:** User required more sophisticated idea capture system supporting various idea types (technical and non-technical) and idea lifecycle management
**Key implementation details:**
- **Two capture modes:**
  - Quick mode: For fleeting ideas (shower ideas, Instagram inspiration) — minimal questioning, fast save
  - Complete mode: For structured technical ideas — adaptive questions based on idea type
- **Idea templates per type:** feature, bug, architecture, refactor, general
- **Status lifecycle:** nova → em progresso → implementada → descartada
- **Sub-commands:**
  - `/grabb list` — list all ideas in projects
  - `/grabb search` — find specific ideas by keywords
  - `/grabb done` — mark ideas as implemented
  - `/grabb review` — review and refine stored ideas
- **Fallback project:** `geral` (general) for non-technical ideas outside code context
- **Storage:** Per-project markdown files in `ideas/` directory with standardized format
- **Skill definition:** SKILL.md fully specifies all behaviors, templates, questions, and command handling
**Status:** Committed to feature branch, ready for testing
**Alternatives rejected:** Single capture mode (now provides flexibility for different idea types)

## 2026-03-25 - Initiate Feature Idea Capture for Bahiarides Project
**Decision:** Began requirements discovery for new demand-capture pages feature for bahiarides website
**Reason:** User invoked `/grabb` skill with feature idea for transportation/mobility website project
**Feature idea:** Create pages to capture demand from people searching for bus services or airport information
**Key requirements under discussion:**
- Potential for two separate pages (bus + airport) OR single page covering both
- Bus page would include links to local bus companies
- Cross-sell integration: link to existing private transfer page for users interested in premium transfers
- Questions to clarify: project name confirmation, scope definition, existing company data, existing transfer page location, priority level
**Status:** Assistant asked clarifying questions to structure the feature; awaiting user responses
**Alternatives rejected:** N/A (requirements gathering phase)

## 2026-03-25 - Pause Bahiarides Feature Discussion for Stakeholder Planning
**Decision:** User indicated need for "more serious conversation" before proceeding with bus company data and implementation details for demand-capture pages
**Reason:** User explicitly stated that further discussion about bus companies and feature scope required preliminary planning conversation; session ended with context restored but discussion not yet conducted
**Key points:**
- Feature requirements partially gathered: 2 separate pages (bus + airport) chosen
- Private transfer page linking confirmed as requirement for bus page
- Bus company data and implementation approach awaiting deeper discussion
- User signaled this is strategic/important conversation, not just technical requirements gathering
**Status:** Paused awaiting user to initiate "serious conversation" about approach/strategy
**Alternatives rejected:** Continue with technical requirements gathering without strategic alignment

## 2026-03-25 - Discovered Repository Structure: Web Agency Workspace
**Decision:** Clarified that `lugga1s/5556` is a web agency workspace with multiple clients, not a skill/feature-only repository
**Reason:** User provided information about repository structure during Bahiarides feature discussion; confirmed agency model with clients in `clients/` directory
**Key points:**
- Repository contains multiple client projects: Bomdelle Confeitaria (HTML code complete), Bahiarides (in development, no code yet)
- Structure: `clients/[client-name]/` directory organization for each client project
- This is the agency's main workspace for web development projects
- Bahiarides demand-capture pages are planned as part of Bahiarides client project development
**Status:** Repository context clarified for future development planning
**Alternatives rejected:** N/A (informational clarification)

## 2026-03-25 - Test and Deploy `/grabb` Skill in Active Session
**Decision:** `/grabb` skill tested in live session during Bahiarides feature idea capture
**Reason:** Opportunity to use the newly implemented `/grabb` skill in a real workflow scenario
**Key points:**
- Skill functioned as designed: detected feature idea, asked clarifying questions, presented workflow
- Demonstrated multi-question flow in Portuguese for feature clarification
- Successfully captured initial requirements about bus/airport demand pages
- Skill paused when user indicated need for strategic discussion before continuing
**Status:** Skill validated and working in production
**Alternatives rejected:** N/A (validation testing)

## 2026-03-25 - Update Memory Files and Commit to Feature Branch
**Decision:** Explicitly updated memory files with new information about agency structure and Bahiarides context
**Reason:** End-of-session memory persistence to ensure agency workspace context is available in future sessions
**Key points:**
- User is running a web agency (not solo development)
- Multiple clients managed with directory organization
- Bahiarides is a client project (transportation/travel service) requiring demand-capture pages
- Committed changes to feature branch `claude/bus-airport-demand-page-9AKhC`
**Status:** Memory files updated and pushed to remote
**Alternatives rejected:** Leave memory files without explicit agency context clarification

## 2026-03-25 - Analyze Session Efficiency: `/grabb` Context Loading Strategy
**Decision:** Identified inefficiency in how memory/context is loaded when `/grabb` skill is invoked
**Reason:** User noticed that `/grabb` sessions trigger reading all 4 memory files + full repository exploration (~27 tool calls, 45k tokens) even when only minimal project context is needed
**Key findings:**
- Current CLAUDE.md instructs reading all 4 memory files at session start (reasonable for development sessions, problematic for `/grabb` idea capture)
- When user asked exploratory question ("tem referência ao bahiarides?"), assistant launched heavy sub-agent exploration unnecessarily
- `/grabb` sessions could be optimized to read only `memory/user.md` initially (to identify active project) instead of full memory dump
**Proposed optimization:**
- Add conditional instruction in CLAUDE.md: When session starts via `/grabb`, read only `memory/user.md` (for project context)
- Skip reading other memory files and avoid full repository exploration unless explicitly asked
- This would dramatically reduce token cost for idea-capture workflows
**Status:** Analysis phase — awaiting user feedback before implementation
**Alternatives rejected:** Implement changes without discussing optimization strategy first

## 2026-03-26 - Detailed Analysis of Session Context Loading Inefficiency
**Decision:** Conducted detailed analysis of how context is loaded during sessions to identify optimization opportunities
**Reason:** User wanted to understand the actual cost (tool calls, tokens) of `/grabb` sessions and how CLAUDE.md instructions drive unnecessary context loading
**Key findings:**
- CLAUDE.md always instructs reading 4 memory files at session start: `memory/user.md`, `memory/preferences.md`, `memory/decisions.md`, `memory/people.md`
- When `/grabb` skill is invoked, full repository exploration follows: 27 tool calls, ~45k tokens spent just on repository scanning
- When user asks contextual questions (e.g., "tem referência ao bahiarides?"), additional heavy exploration is triggered unnecessarily
- Current behavior: Memory files read first (before `/grabb` is even processed) → skill triggers → exploratory context loading → idea capture
- Ideal behavior: Read only `memory/user.md` for project identification → proceed directly to idea capture workflow
**Proposed solution:**
- Create conditional instructions in CLAUDE.md: "When session initiated via `/grabb`, read only `memory/user.md` for project context. Skip other memory files and repository exploration unless explicitly requested."
- This would reduce `/grabb` session cost from ~45k tokens to minimal footprint
**Considerations:**
- Stop hook behavior and how it determines which project to save ideas to (depends on context available in session)
- When to allow full context loading (e.g., user asks for Bahiarides context: should it trigger exploration or use memory files?)
**Status:** Analysis complete, proposed optimization strategy identified, awaiting final decision on implementation
**Alternatives rejected:** Leave current behavior unchanged (wastes token budget on routine idea-capture workflows)

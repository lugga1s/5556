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

## 2026-03-25 - Initiate Capture of First Idea: Bahiarides Baby Seat Feature
**Decision:** User invoked `/grabb` skill to capture first idea for new "Bahiarides" project
**Reason:** User is actively testing the `/grabb` skill they built and capturing real project ideas
**Key details:**
- **Project:** Bahiarides (new ride-sharing or transportation service)
- **Idea:** Informar disponibilidade de cadeirinha para bebê nos carros (Inform availability of baby seats in cars)
- **Type:** feature
- **Priority:** média (medium)
- **Status:** nova (new)
- **Context:** First idea captured for Bahiarides project; skill is functioning as designed; user waiting for confirmation before save
**Status:** Awaiting user confirmation to save to `ideas/bahiarides.md`
**Alternatives rejected:** N/A (feature in use)

## 2026-03-25 - Clarify Idea Type Classifications in `/grabb` Skill
**Decision:** Explained all 5 idea type classifications available in `/grabb` skill with Bahiarides examples
**Reason:** User asked for clarification on idea types (didn't understand the difference between feature, component, design, etc.)
**Key details:**
- **`feature`** — Nova funcionalidade para o usuário (new user-facing functionality); Ex: motoristas podem indicar que têm cadeirinha no carro
- **`bug`** — Algo que está quebrado ou errado (broken/incorrect behavior); Ex: mapa não atualiza posição do motorista em tempo real
- **`arquitetura`** — Mudança estrutural/sistêmica (system-level structural changes); Ex: migrar autenticação para JWT, separar serviço de pagamento em microserviço
- **`refactor`** — Melhoria de código sem mudar comportamento (code improvement without changing user-visible behavior); Ex: reescrever módulo de rotas para reduzir duplicação
- **`geral`** — Ideia não-técnica, inspiração, aprendizado (non-technical ideas, inspiration, learning); Ex: parceria com hotéis da Bahia, ideia de campanha de marketing
- **Baby seat idea classification:** `feature` — because it's a new user-visible functionality (displaying availability info in app/site)
**Alternatives rejected:** N/A (clarification/education only)


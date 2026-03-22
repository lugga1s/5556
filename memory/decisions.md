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

## 2026-03-22 - Install ui-ux-pro-max-skill Design Package
**Decision:** Install comprehensive design skills package from GitHub (nextlevelbuilder/ui-ux-pro-max-skill)
**Reason:** User requested acquiring the ui-ux-pro-max-skill to extend project with professional design and UI/UX capabilities
**Key points:**
- Successfully cloned repository and integrated 7 new design-related skills
- New skills: ui-ux-pro-max (main design intelligence with 161 rules, 67 styles, 161 palettes), design, design-system, ui-styling, banner-design, brand, slides
- In addition to existing skill-creator, project now has 8 total custom skills
- Skills provide coverage for 50+ design styles, 161 color palettes, 57 font pairings, 161 product type patterns, 99 UX guidelines across 10 tech stacks
**Alternatives rejected:** Maintain project without comprehensive design capabilities

## 2026-03-22 - Commit and Push Design Skills Installation
**Decision:** Committed 190 files containing installed design skills and pushed to feature branch `claude/add-memory-features-k6bgv`
**Reason:** Persist the newly installed ui-ux-pro-max-skill package and 7 design-related skills to version control
**Key points:**
- All design skill directories committed: banner-design, brand, design, design-system, slides, ui-styling, ui-ux-pro-max
- 190 files total added to git
- Pushed to feature branch (not yet merged to master)
- Skills are now persistent in repository structure at `.claude/skills/`
**Alternatives rejected:** Leave skills untracked (would lose them if directory is deleted or cloned fresh)

## 2026-03-22 - Confirm Persistent Memory Functionality Across Sessions
**Decision:** User tested and verified that persistent memory system works correctly between sessions
**Reason:** User initiated new session and confirmed that memory files from previous session were preserved and accessible
**Key points:**
- User's first question in new session: "então agora você tem memória, o que lembra?" (now you have memory, what do you remember?)
- Memory files successfully persisted: user.md, preferences.md, decisions.md, people.md all contained previous session information
- Persistent memory system is functioning as designed and users can rely on context preservation across sessions
**Alternatives rejected:** N/A (validation and confirmation)

## 2026-03-22 - Acquire ui-ux-pro-max-skill Design Package from GitHub
**Decision:** User requested and acquired comprehensive design skills package from nextlevelbuilder/ui-ux-pro-max-skill GitHub repository
**Reason:** Extend project with professional-grade design and UI/UX capabilities across multiple platforms and frameworks
**Key points:**
- Source: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- 7 new design-related skills installed: ui-ux-pro-max (main), design, design-system, ui-styling, banner-design, brand, slides
- ui-ux-pro-max includes: 161 color palettes, 57 font pairings, 50+ visual styles, 161 product type patterns, 99 UX guidelines
- Skills support 10 technology stacks: React, Next.js, Vue, Svelte, SwiftUI, React Native, Flutter, Tailwind, shadcn/ui, HTML/CSS
- 190 files committed and pushed to repository
- Project now has 8 total custom skills (skill-creator + 7 design skills)
**Alternatives rejected:** Maintain project without comprehensive design capabilities

## 2026-03-22 - Explore New Design Capabilities
**Decision:** User expressed intent to explore new design skills and integrate them into future work
**Reason:** Understand the capabilities and practical applications of the newly installed design skills
**Key points:**
- User stated: "vamos explorar suas novas habilidades e oportunidades a partir de agora" (let's explore your new capabilities and opportunities from now on)
- Design skills are now available for immediate use in design/UI projects
- Skills provide ready-made intelligence for color selection, typography, UX patterns, component design, branding, presentations
**Alternatives rejected:** N/A (exploratory phase initiated)

## 2026-03-22 - Create Banner Showcase for Cookie & Co. Client Project
**Decision:** Create 3 banner design variations for "Cookie & Co." client project using banner-design skill
**Reason:** Put newly installed design skills into practical use; create client deliverables with multiple style options
**Key points:**
- Created responsive HTML banner showcase: `/assets/banners/cookie-co/showcase.html`
- 3 design variations: (1) Retro/Vintage with decorative borders and serif typography, (2) Illustrated/Playful with CSS animations and organic shapes, (3) Gradient/Bold Typography with premium aesthetic
- Followed banner-design skill rules: critical content in 70-80% safe zone, max 2 fonts, contrast ≥ 4.5:1, one CTA per banner
- All banners are responsive and adapt to any screen width
- Deliverable format: single HTML file with showcased variations, ready for client review
**Alternatives rejected:** Static image deliverables (HTML provides better interactivity and responsiveness)

## 2026-03-22 - Clarify Claude Code Artifact Limitations vs Interactive Preview
**Decision:** Explained that Claude Code (CLI) cannot create interactive artifacts like Claude.ai (web) but offers local HTTP server as alternative
**Reason:** User asked about viewing interactive artifact previews; needed to clarify platform differences
**Key points:**
- Claude.ai web interface: Can render interactive artifacts directly in browser
- Claude Code CLI: Output limited to text, files, and code — cannot render interactive browser artifacts
- Alternative solutions: (1) Open HTML files directly in browser manually, (2) Start local HTTP server to serve files and view via `localhost`
- File already created and ready to open: `/home/user/5556/assets/banners/cookie-co/showcase.html`
**Alternatives rejected:** N/A (clarification of platform capabilities only)


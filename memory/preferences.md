# Preferences

<!-- Update this file with the user's preferences: communication style, tools, languages, formatting, etc. -->

## Communication Style
- Prefers Portuguese (Brazilian Portuguese) for conversation
- Prefers Portuguese for technical discussions and requirements interviews (e.g., idea-capture skill design)
- Asks clarifying questions about system behavior
- Makes conceptual and reflexive questions about how systems work
- Prefers understanding the "why" before the "how"
- Values transparency in how assistant filters and decides what to persist in memory

## Tools & Technologies
- Uses GitHub/Git for version control
- Works with Claude Code CLI

## Code Style


## Skill Development Approach
- Prefers understanding the 3 skill creation patterns before building
- Values having detailed requirements interviews before implementation
- Interested in learning about trigger optimization and skill lifecycle
- Uses `/grabb` skill actively for feature idea capture during development work
- Integrates skill use into real project workflows

## Other Preferences
- Interested in understanding automation and when memory updates happen automatically
- Prefers to understand system behavior and configuration (asking about Stop hook and auto-registration)
- Appreciates learning how the Stop hook and persistent memory system work
- Curious about end-of-session triggers and what causes automatic updates
- Values understanding the **why** behind system behavior before implementation details
- Interested in learning filtering criteria for what constitutes persistent vs. contextual information
- Wants to understand the assistant's filtering logic for what information should be persisted
- Interested in skill creation and Claude Code extensibility (reviewed Skill Creator documentation)
- Active in installing and setting up development tools in the repository (.claude/skills/ structure)
- Interested in understanding Claude Code Skills system (SKILL.md format, triggering mechanisms, real-world applications)
- Values learning about enterprise use cases and business impact analysis of AI tools
- Interested in creating a custom skill for capturing/managing ideas with project organization
- Prefers asking clarifying questions before implementing features (seeks detailed requirements)
- Curious about why certain tools (like Skill Creator) are not globally available vs. project-local
- Interested in the practical workflow of skill creation: interview → draft → test → feedback → iteration
- Researches real-world business impact and use cases of tools before deciding on adoption
- Prefers understanding enterprise patterns (e.g., TELUS 13k skills, Rakuten 12.5M line codebase)
- Interested in understanding the ease/benefit of using Skill Creator to build new skills
- Wants to understand minimum context required to effectively use Skill Creator for new skill development
- Curious about differences in skill availability between web/sandbox environment vs. local machine
- Interested in understanding skill scopes: global (`~/.claude/skills/`) vs. project-specific (`.claude/skills/`)
- Actively explores and discovers system features (e.g., discovered persistent memory system and asked clarifying questions about how it works)
- Interested in Git workflow and PR approval processes for feature branches
- Values understanding the relationship between branch strategy and memory/persistence in feature branches
- Curious about Claude Code visual indicators (e.g., `diff +3000`, `PR`) and what they represent in context processing
- Interested in understanding internal metrics of token/context usage during response generation
- Aware that offline PC affects ability to copy/sync files locally to global scopes
- Interested in using Gitea API for creating PRs when `gh` CLI is not available
- Prefers practical explanations using metaphors (e.g., Git as physical file cabinet with drawers for branches)
- Appreciates layered explanations: technical details + layperson analogies for understanding complex concepts
- Actively checks memory state to recall project goals and previous decisions
- Expects memory persistence to work reliably across sessions for ongoing projects
- Prefers natural language triggers in Portuguese for skill activation (e.g., "anota essa ideia", "salva isso", "grava essa ideia")
- Values confirmation workflow before saving ideas (show summary, ask permission, then commit)
- Prefers project-based organization of ideas (one markdown file per project in `ideas/` directory)
- Interested in chatGPT-like workflow for idea capture: interactive questioning → summary confirmation → automatic save
- Uses metaphors and analogies to understand technical concepts (e.g., comparing SKILL.md to an "OS" or "brain" of the skill)
- Asks clarifying questions to confirm understanding after implementation is complete (verifies architecture/structure comprehension)
- Verifies skill functionality through direct testing and conceptual confirmation questions
- Uses plan mode strategically to prevent unwanted auto-commits while developing features
- Appreciates iterative enhancement of skills based on refined requirements and feature requests
- Values dual-mode workflows: quick capture for ephemeral ideas, complete capture for structured technical ideas
- Prefers adaptive questioning based on context (idea type, complexity, project)
- Uses `/grabb` skill with natural language Portuguese triggers to capture feature ideas from projects
- Interested in optimizing efficiency of sessions — wants to reduce unnecessary file reads and API calls
- Prefers analyzing system behavior before making changes (requests discussion and analysis first, then implementation)
- Concerned about token efficiency when using `/grabb` — doesn't want the entire repository to be read when only project context is needed
- Values transparency about what context/files are loaded during sessions (wants to understand CLAUDE.md instructions impact)
- Interested in conditional logic for CLAUDE.md instructions based on session trigger type (e.g., different behavior for `/grabb` vs regular sessions)
- Interested in understanding Stop hook implementation details: memory persistence vs git verification mechanisms
- Values understanding system dependencies and architectural problems before implementing solutions
- Prefers discussing and analyzing options before implementing improvements to complex systems
- Interested in understanding the architecture and problems with the current Stop hook system before proposing fixes
- Appreciates detailed breakdowns of system components and their interactions
- Interested in understanding whether current system behaviors are intentional or problematic


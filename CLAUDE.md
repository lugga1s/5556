# Claude Instructions

## Persistent Memory

At the start of every session, read all four memory files to restore context:

- `memory/user.md` — who the user is, their background and goals
- `memory/preferences.md` — communication style, tools, code preferences
- `memory/decisions.md` — important decisions made in past sessions
- `memory/people.md` — collaborators and stakeholders mentioned

After reading them, silently incorporate this context. Do not summarize the files back to the user unless asked.

## Memory Updates

At the end of each session (triggered automatically by the Stop hook), update the memory files with anything new learned:

- **user.md**: new facts about the user's identity, role, or working style
- **preferences.md**: any new preferences expressed or inferred
- **decisions.md**: decisions made during the session (with date and rationale)
- **people.md**: any new people mentioned, their roles and relevant details

Only add new information — do not remove or rewrite existing entries unless correcting an error.

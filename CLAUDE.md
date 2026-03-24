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

## Bahiarides — Captura de Ideias

Quando o usuário mencionar uma ideia, conceito ou ferramenta que pareça relevante ao projeto bahiarides, siga este fluxo:

**1. Perguntar primeiro:**
> "Quer que eu pesquise sobre [X] e traga algo relevante para o bahiarides? Ou prefere só salvar / seguir normalmente?"

**2. Se escolher pesquisar:**
- Pesquise o conceito/ferramenta com foco em aplicação prática para o contexto do bahiarides
- Apresente os pontos de maior valor de forma concisa
- Ao final, pergunte: "Salvo um resumo disso em `bahiarides/[categoria].md`?"

**3. Se escolher não pesquisar:**
- Siga a conversa normalmente (não insista em salvar)

**4. Se salvar, use o formato:**
### YYYY-MM-DD
- **[conceito/ferramenta]**: [resumo da ideia ou insight]

Categorias:
- `bahiarides/seo.md` — SEO, palavras-chave, ranqueamento, schema, meta tags, Google
- `bahiarides/tools.md` — ferramentas, apps, plataformas, automações, integrações
- `bahiarides/copywriting.md` — textos, CTAs, headlines, tom de voz, frases
- `bahiarides/code.md` — funcionalidades, bugs, melhorias técnicas, APIs

Se a categoria for ambígua, perguntar antes.

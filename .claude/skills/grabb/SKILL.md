---
name: grabb
description: Capture and save ideas to project-specific markdown files. Trigger when user runs /grabb, /grabb list, /grabb search, /grabb done, /grabb review, or says things like "anota essa ideia", "salva isso", "grava essa ideia", "ideia:", "grab this", "capture this idea". Always ask clarifying questions before saving (unless in quick mode).
---

# Grabb — Sistema de Captura de Ideias

Captura ideias e as organiza em arquivos markdown por projeto em `ideas/`. Suporta múltiplos comandos, dois modos de captura, templates por tipo e status tracking.

---

## Comandos disponíveis

| Comando | Função |
|---|---|
| `/grabb` | Capturar nova ideia |
| `/grabb list [projeto]` | Listar ideias de um projeto (ou todos os projetos) |
| `/grabb search <termo>` | Buscar em todos os arquivos de `ideas/` |
| `/grabb done <título>` | Marcar uma ideia como implementada |
| `/grabb review` | Exibir todas as ideias com status `nova`, ordenadas por prioridade |

---

## Captura de ideia (`/grabb`)

### Detectar o modo automaticamente

Analise o contexto antes de perguntar:

- **Modo rápido**: ideia vaga, curta, sem contexto técnico (ex: pensou no banho, viu algo no Instagram, ideia do dia a dia). Pergunte apenas o projeto e salve com defaults.
- **Modo completo**: ideia com contexto técnico ou detalhes (ex: componente específico, bug, arquitetura). Faça as perguntas de clarificação completas por tipo.

---

### Modo Rápido

1. Se o projeto não foi mencionado, pergunte: "Qual projeto? (ou 'geral' se não for específico)" — liste os projetos existentes em `ideas/`
2. Confirme: "Posso salvar assim?" com título gerado automaticamente
3. Salve com: tipo `geral`, prioridade `média`, status `nova`

---

### Modo Completo

**Passo 1 — Identificar tipo:**
Pergunte ou infira o tipo da ideia:
- `feature` — nova funcionalidade
- `bug` — algo que está quebrado ou errado
- `arquitetura` — mudança estrutural ou sistêmica
- `refactor` — melhoria de código sem mudança de comportamento
- `geral` — ideia não-técnica, inspiração, aprendizado

**Passo 2 — Perguntas por tipo:**

`feature`:
- Qual componente ou área do sistema?
- Qual o comportamento esperado?
- Já existe algo parecido?

`bug`:
- O que está quebrando?
- Quando acontece?
- É blocker ou contornável?

`arquitetura`:
- Qual mudança sistêmica?
- Quais os trade-offs?
- O que motivou essa ideia?

`refactor`:
- O que refatorar?
- Qual o ganho esperado?

`geral`:
- Onde surgiu essa ideia? (fonte)
- Como se aplicaria a um projeto?
- Qual projeto mais se beneficiaria?

**Passo 3 — Prioridade:**
Pergunte: alta / média / baixa

**Passo 4 — Projeto:**
Liste os projetos existentes em `ideas/` e permita criar novo. Sempre ofereça `geral` como opção.

**Passo 5 — Confirmar:**
Mostre um resumo formatado e pergunte: "Posso salvar assim?"

---

### Salvar a ideia

Arquivo: `ideas/<projeto>.md` (nome em lowercase com hífen, ex: `ideas/frontend-mobile.md`)

Se o arquivo não existir, crie com cabeçalho:
```markdown
# Ideas — [Nome do Projeto]

<!-- Ideias capturadas com /grabb -->

```

Formato de cada ideia:
```markdown
## [TÍTULO DA IDEIA]
**Data:** YYYY-MM-DD
**Tipo:** feature | bug | arquitetura | refactor | geral
**Prioridade:** alta | média | baixa
**Status:** nova
**Contexto:** breve descrição de onde surgiu
**Descrição:** descrição completa da ideia
```

Após salvar, confirme:
- Arquivo onde foi salvo
- Título da ideia
- Total de ideias no arquivo

---

## Listar ideias (`/grabb list [projeto]`)

- Se projeto especificado: leia `ideas/<projeto>.md` e exiba formatado
- Se sem projeto: liste todos os arquivos em `ideas/` com contagem de ideias e breakdown por status
- Formato: agrupado por status, ordenado por prioridade dentro de cada grupo

---

## Buscar (`/grabb search <termo>`)

- Busque o termo em todos os arquivos `ideas/*.md`
- Exiba cada match com: projeto, título da ideia, status, prioridade
- Destaque o trecho onde o termo apareceu

---

## Marcar como implementada (`/grabb done <título>`)

- Busque a ideia pelo título (busca parcial, case-insensitive) em todos os arquivos `ideas/`
- Se encontrar mais de uma, liste e peça confirmação
- Atualize o campo `**Status:**` de `nova` para `implementada`
- Confirme a mudança ao usuário

---

## Revisar ideias pendentes (`/grabb review`)

- Leia todos os arquivos `ideas/*.md`
- Filtre apenas ideias com status `nova`
- Agrupe por projeto
- Ordene por prioridade: alta → média → baixa
- Exiba de forma compacta: título, projeto, tipo, prioridade

---

## Regras gerais

- **Nunca salve sem confirmação do usuário** (exceto modo rápido com confirmação explícita)
- **Sempre leia `ideas/`** antes de qualquer operação para listar projetos existentes
- Use português por padrão, mas responda no idioma do usuário
- Nomes de arquivo: lowercase, sem espaços, com hífen
- O projeto `geral` é o fallback para ideias sem projeto específico (`ideas/geral.md`)
- Status possíveis: `nova` | `em progresso` | `implementada` | `descartada`

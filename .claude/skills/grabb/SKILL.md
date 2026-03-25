---
name: grabb
description: Capture and save ideas to project-specific markdown files. Trigger when user runs /grabb, or says things like "anota essa ideia", "salva isso", "grava essa ideia", "ideia:", "grab this", "capture this idea". Always ask clarifying questions before saving.
---

# Grabb — Idea Capture Skill

Captura ideias do usuário e salva em arquivos markdown organizados por projeto em `ideas/`.

## Comportamento

Quando ativado (via `/grabb` ou linguagem natural), siga **sempre** este fluxo:

### 1. Extrair ideia inicial

Se o usuário já passou uma ideia junto com o comando (ex: `/grabb quero um componente de modal reutilizável`), use isso como ponto de partida. Caso contrário, peça para descrever a ideia brevemente.

### 2. Fazer perguntas de clarificação

Sempre faça as seguintes perguntas antes de salvar (pode ser em uma mensagem só, de forma conversacional):

1. **Projeto**: A qual projeto essa ideia pertence? Liste os projetos existentes lendo os arquivos em `ideas/` e ofereça as opções. Permita criar um novo projeto.
2. **Contexto**: Em que situação surgiu essa ideia? (ex: "estava implementando X e pensei em Y")
3. **Prioridade**: É urgente, médio prazo, ou só uma ideia futura? (alta / média / baixa)
4. **Já existe algo parecido?**: Pergunte brevemente se é algo novo ou uma melhoria de algo existente.

Se alguma resposta já estiver clara pelo contexto da conversa, não repita a pergunta — use o que já foi dito.

### 3. Confirmar antes de salvar

Mostre um resumo formatado da ideia e pergunte: "Posso salvar assim?" Só salve após confirmação.

### 4. Salvar em `ideas/<projeto>.md`

Formato de cada ideia no arquivo:

```markdown
## [TÍTULO DA IDEIA]
**Data:** YYYY-MM-DD
**Prioridade:** alta | média | baixa
**Contexto:** breve descrição de onde surgiu
**Descrição:** descrição completa da ideia
**Status:** nova
```

Se o arquivo do projeto não existir, crie-o com um cabeçalho:

```markdown
# Ideas — [Nome do Projeto]

<!-- Ideias capturadas com /grabb -->

```

Append a nova ideia ao final do arquivo.

### 5. Confirmar ao usuário

Após salvar, mostre:
- Arquivo onde foi salvo
- Título da ideia
- Número total de ideias no arquivo

---

## Regras

- **Nunca salve sem confirmação do usuário**
- **Sempre leia `ideas/` antes** para listar projetos existentes e evitar duplicatas de nome de arquivo
- Use português por padrão, mas responda no idioma do usuário
- Títulos de arquivo: lowercase, sem espaços, com hífen (ex: `ideas/frontend-mobile.md`)
- Se o usuário quiser listar ideias de um projeto, leia e exiba o arquivo correspondente de forma formatada

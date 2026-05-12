# Skills

Repositório de skills locais para orientar agentes/LLMs em tarefas específicas.

## Skills disponíveis

- [code-smell](skills/code-smell/README.md) — Catálogo de code smells em TypeScript/JavaScript para revisão, refatoração e discussão de qualidade de código.
- [symlinks](skills/symlinks/README.md) — Guia para criar links simbólicos de `.agents/skills` para pastas de skills usadas por outros agentes locais.

## Estrutura geral

Cada skill fica em `skills/<nome-da-skill>/` e segue o padrão:

```text
SKILL.md   # Entrada principal para agentes
README.md  # Resumo para humanos
AGENTS.md  # Guia detalhado para agentes/LLMs, quando necessário
```

# Skills

Repositório de skills locais para orientar agentes/LLMs em tarefas específicas.

## Skills disponíveis

### Angular

- [angular-developer](skills/angular-developer/SKILL.md) — Geração de código Angular e orientação arquitetural: reatividade (signals, linkedSignal, resource), forms, injeção de dependência, rotas, SSR, acessibilidade (ARIA), animações, estilos, testes e CLI.
- [angular-folder-structure](skills/angular-folder-structure/SKILL.md) — Padroniza a estrutura de pastas de uma aplicação Angular (organização por feature sob `src/app`, componentes standalone, arquivos co-localizados, rotas lazy-loaded e nomes hifenizados).
- [angular-http](skills/angular-http/SKILL.md) — Cria módulos de integração HTTP padronizados (serviços tipados com `HttpClient`, DTOs com `@Dto()`, serviço mock e testes unitários) na pasta `services/http` mais próxima do consumidor.
- [angular-patterns](skills/angular-patterns/SKILL.md) — Aplica boas práticas atuais de Angular ao escrever ou revisar componentes, templates, reatividade e forms (standalone, signals, OnPush/zoneless, control flow nativo, `inject()`, forms reativos tipados).

### TypeScript

- [typescript-patterns](skills/typescript-patterns/SKILL.md) — Padrões TypeScript da casa: nomes de arquivo kebab-case, uma declaração por arquivo com sufixo de tipo, classes para modelos de domínio, interfaces com prefixo `I`, enums para domínios fechados e teste unitário para cada unidade.
- [code-standards-en](skills/code-standards-en/SKILL.md) — Convenções de identificadores e código em inglês: casing, funções iniciadas por verbo, objetos parametrizados, separação CQS, early returns guardados e limites de tamanho de métodos/classes.

### Qualidade e testes

- [code-smell](skills/code-smell/SKILL.md) — Catálogo de code smells em TypeScript/JavaScript para revisão, refatoração e discussão de qualidade de código.
- [vitest-testing](skills/vitest-testing/SKILL.md) — Orienta testes unitários e de integração com Vitest: mocks `vi`, Arrange-Act-Assert, fake timers para `Date` e testes de integração de endpoints HTTP sem supertest.

### Ferramentas e utilidades

- [context7](skills/context7/SKILL.md) — Recupera documentação técnica atualizada, referências de API e exemplos de código para qualquer tecnologia de desenvolvimento. Inspirado no projeto [pedronauck/skills](https://github.com/pedronauck/skills).

## Estrutura geral

Cada skill fica em `skills/<nome-da-skill>/` e segue o padrão:

```text
SKILL.md      # Entrada principal e única para agentes (frontmatter: name + description)
references/   # Contexto carregado sob demanda (progressive disclosure), um nível de profundidade
scripts/      # Scripts CLI determinísticos, quando necessário
assets/       # Templates e arquivos estáticos, quando necessário
```

As skills seguem a especificação [agentskills.io](https://agentskills.io): metadados validados,
`SKILL.md` enxuto (< 500 linhas) e detalhes movidos para `references/`.

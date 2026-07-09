# Skills

Repositório de skills para orientar agentes/LLMs em tarefas específicas.

## Instalação

As skills são instaladas com o CLI [`skills`](https://skills.sh) (sem necessidade de
instalar nada globalmente):

```bash
# Instala todas as skills do repositório
npx skills add gilvam/skills

# Instala apenas skills específicas
npx skills add gilvam/skills --skill angular-http --skill typescript-patterns
```

O CLI detecta o agente em uso e copia as skills para o diretório correto do projeto
(`.claude/skills/` para Claude Code, `.agents/skills/` para Cursor/Codex/universais).
O `skills-lock.json` é gravado no **seu** projeto, registrando origem e hash de cada skill.

### Atualizando

Para puxar as versões mais recentes das skills já instaladas:

```bash
npx skills update            # verifica e atualiza (interativo)
npx skills update -y         # não-interativo
```

## Skills disponíveis

### Angular

- [angular-developer](skills/angular-developer/SKILL.md) — Geração de código Angular e orientação arquitetural em toda a stack: componentes/diretivas/pipes/serviços, `ng generate`/`ng new`/`ng build`, reatividade (signals, linkedSignal, resource, effect), forms, DI, rotas e guards, SSR/hydration, acessibilidade (Angular Aria), animações, estilos, testes unitários/e2e e CLI/migrations.
- [angular-folder-structure](skills/angular-folder-structure/SKILL.md) — Estrutura e padroniza a árvore de pastas Angular ao iniciar um projeto (`ng new`), organizar uma feature ou decidir onde um arquivo/componente/serviço vive (`core`/`_shared`/`features`/`pages`).
- [angular-http](skills/angular-http/SKILL.md) — Cria/padroniza módulos de integração HTTP (serviços tipados com `HttpClient`, DTOs com `@Dto()`, mock service e testes) na pasta `services/http` mais próxima do consumidor.
- [angular-patterns](skills/angular-patterns/SKILL.md) — Aplica boas práticas atuais de Angular ao escrever/revisar componentes, diretivas, pipes e templates, migrar padrões legados (`*ngIf`/`*ngFor`, `NgModule`, injeção via construtor) para os atuais, modelar estado (signals vs RxJS) ou montar forms reativos.

### TypeScript

- [typescript-patterns](skills/typescript-patterns/SKILL.md) — Padrões TypeScript da casa ao criar/revisar um `.ts`: nomes kebab-case, uma declaração por arquivo com sufixo de tipo, classes com campos default-initialized, interfaces `I`-prefixadas, enums para domínios fechados, métodos de array em vez de loop e teste unitário por unidade.
- [code-standards-en](skills/code-standards-en/SKILL.md) — Convenções de nomenclatura e estrutura ao nomear/estruturar função ou classe e revisar PRs: identificadores em inglês, verbo-first, parâmetros como objeto, CQS, early returns, magic numbers extraídos e limites de tamanho.

### Qualidade e testes

- [code-smell](skills/code-smell/SKILL.md) — Catálogo de 94 code smells em TypeScript/JavaScript para nomear o problema (método longo, muitos parâmetros, duplicação...) e justificar a refatoração via SOLID/Clean Code ao revisar, refatorar ou escrever código.
- [vitest-testing](skills/vitest-testing/SKILL.md) — Orienta testes unitários e de integração com Vitest ao criar/revisar um `.spec.ts`/`.test.ts`: mocks `vi`, Arrange-Act-Assert, fake timers para `Date` e testes de endpoints HTTP sem supertest.

### Ferramentas e utilidades

- [context7](skills/context7/SKILL.md) — Recupera documentação técnica atualizada, referências de API e exemplos de código para qualquer tecnologia de desenvolvimento. Inspirado no projeto [pedronauck/skills](https://github.com/pedronauck/skills).

### Autoria de skills

- [skill-best-practices](skills/skill-best-practices/SKILL.md) — Cria e estrutura skills de agente seguindo agentskills.io ao criar um novo diretório de skill, redigir instruções procedurais ou otimizar uma `description` para descoberta. Inspirado no projeto [rodrigobranas](https://github.com/rodrigobranas).

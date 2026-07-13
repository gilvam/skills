# Skills → ações

Consulte o `SKILL.md` em `skills/<nome>/` antes de criar, editar, refatorar ou revisar. Cada skill
traz seus próprios gatilhos, regras não negociáveis e material de apoio em `references/`.

| Skill                         | Acionar para…                                                                                                                                                                                                                                           | Não usar se…                                                                                                                          |
| ------------------------------ |---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `angular-developer`            | Criar, editar ou refatorar projeto/componente/diretiva/pipe/serviço Angular (novo ou existente); `ng generate`/`ng new`/`ng build`; arquitetura ampla (signals, forms, DI, routing, SSR, ARIA, animações, CLI/migrations, testes)                       | Regras de pastas (`angular-folder-structure`), HTTP (`angular-http`) ou padrão de componente (`angular-patterns`)                     |
| `angular-folder-structure`     | Iniciar, organizar, editar ou reestruturar projeto ou feature Angular (novo ou existente); decidir onde um arquivo/componente/serviço vive (`core`/`_shared`/`features`/`pages`)                                                                        | Módulo HTTP (`angular-http`) ou lógica de componente/UI (`angular-patterns`)                                                          |
| `angular-http`                 | Criar, padronizar, editar ou refatorar chamada REST: serviço `HttpClient` tipado, DTOs `@Dto()`, mock service, specs (novo ou existente)                                                                                                                | Lógica de UI/componente (`angular-patterns`) ou layout de pastas (`angular-folder-structure`)                                         |
| `angular-patterns`             | Criar, editar, refatorar ou revisar componente, diretiva, pipe, template (novo ou existente); migrar padrão legado (`*ngIf`/`*ngFor`, `NgModule`, injeção via construtor) para o atual; modelar estado (signals/RxJS/async-await) ou formulário reativo | Layout de pastas (`angular-folder-structure`) ou módulo HTTP (`angular-http`)                                                         |
| `code-smell`                   | Criar, editar, refatorar ou revisar TS/JS (novo ou existente, não só review) — nomear problema de design (método longo, muitos parâmetros, duplicação, acoplamento) e justificar refatoração; antes de reportar concluído/abrir PR                      | Detectar bug funcional (testes/type-checker), medir performance (profiling/benchmarks) ou aplicar estilo automático (ESLint/Prettier) |
| `code-standards-en`            | Criar, editar, refatorar ou revisar (não só review) — nomear/renomear função, variável, classe ou arquivo; extrair magic number; separar boolean-flag; inglês, verbo-first, CQS, early return, tamanho; antes de reportar concluído/abrir PR            | Política do produto exige identificadores localizados (não inglês)                                                                    |
| `context7`                     | Consultar documentação/API atualizada de qualquer lib, framework, SDK ou CLI antes de codar                                                                                                                                                             | Código interno deste repositório ou conteúdo não técnico                                                                              |
| `skill-best-practices`         | Criar ou revisar uma skill deste repositório: pastas `scripts/`/`references/`/`assets/`, metadados, `description`                                                                                                                                       | Documentação geral, README ou código de lib que não seja uma skill                                                                    |
| `typescript-patterns`          | Criar, editar, refatorar ou revisar `.ts` (novo ou existente): nomear arquivo/tipo, modelar classe/interface/enum, valor default de campo, trocar loop por métodos de array                                                                             | Casing de identificador (`code-standards-en`), layout Angular (`angular-folder-structure`), DTOs HTTP (`angular-http`)                |
| `typescript-sonarqube`         | Criar, editar, refatorar ou revisar (último) de qualquer `.ts`/`.tsx`/`.js`/`.jsx` (novo ou existente) Ou pedido de revisão "Sonar"/"SonarLint"                                                                                                         | Formatação pura de código (Prettier/ESLint) sem relação com regras Sonar                                                              |
| `vitest-testing`               | Escrever/revisar teste Vitest (`.spec.ts`/`.test.ts`): mocks `vi`, AAA, timers, integração HTTP sem supertest                                                                                                                                           | Projeto usa Jest ou Sinon como stack principal de mock                                                                                |

**Ordem sugerida por tarefa:**
- `code-standards-en`, `code-smell` e `typescript-sonarqube` são sempre as **últimas** skills
chamadas — em qualquer tarefa que crie, edite ou refatore código, não só em revisão — nessa
ordem, como fechamento depois da skill de implementação principal.
- **Projeto/feature Angular:** `angular-folder-structure` → `angular-developer` → `angular-patterns`
  → (`angular-http` se houver REST) → `typescript-patterns` → **PORTÃO** (`code-standards-en` →
  `code-smell` → `typescript-sonarqube`).
- **HTTP/REST em Angular:** `angular-http` → `typescript-patterns` → **PORTÃO** (trio).
- **Componente/diretiva/pipe/template (criar ou editar/refatorar):** `angular-patterns` →
  `typescript-patterns` → **PORTÃO** (trio).
- **TypeScript puro (criar/editar/refatorar `.ts`):** `typescript-patterns` → **PORTÃO** (trio).
- **Testes:** `vitest-testing` + skill da camada testada.
- **Nova skill de agente:** `skill-best-practices`.

# Persistência do Modo Plano

<plan_file>`.claude/plans/[timestamp]-[plan-slug].md`</plan_file>

- **OBRIGATÓRIO ABSOLUTO**: No modo Plano, após o usuário aceitar um plano, **SEMPRE** escreva o plano
  aceito em um arquivo Markdown dentro de <plan_file>.
- **OBRIGATÓRIO**: Se o plano aceito for atualizado posteriormente, atualize ou adicione o respectivo
  arquivo Markdown em <plan_file>.

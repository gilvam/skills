# Skills → ações

Consulte o `SKILL.md` em `skills/<nome>/` antes de implementar ou revisar. Cada skill traz seus
próprios gatilhos, regras não negociáveis e material de apoio em `references/`.

| Skill                         | Acionar para…                                                                                                                                                                                                    | Não usar se…                                                                                                                          |
| ------------------------------ |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `angular-developer`            | Criar projeto/componente/diretiva/pipe/serviço Angular; `ng generate`/`ng new`/`ng build`; arquitetura ampla (signals, forms, DI, routing, SSR, ARIA, animações, CLI/migrations, testes)                         | Regras de pastas (`angular-folder-structure`), HTTP (`angular-http`) ou padrão de componente (`angular-patterns`)                     |
| `angular-folder-structure`     | Iniciar/organizar projeto ou feature Angular; decidir onde um arquivo/componente/serviço vive (`core`/`_shared`/`features`/`pages`)                                                                              | Módulo HTTP (`angular-http`) ou lógica de componente/UI (`angular-patterns`)                                                          |
| `angular-http`                 | Criar/padronizar chamada REST: serviço `HttpClient` tipado, DTOs `@Dto()`, mock service, specs                                                                                                                   | Lógica de UI/componente (`angular-patterns`) ou layout de pastas (`angular-folder-structure`)                                         |
| `angular-patterns`             | Escrever/revisar componente, diretiva, pipe, template; migrar padrão legado (`*ngIf`/`*ngFor`, `NgModule`, injeção via construtor) para o atual; modelar estado (signals/RxJS/async-await) ou formulário reativo | Layout de pastas (`angular-folder-structure`) ou módulo HTTP (`angular-http`)                                                         |
| `code-smell`                   | Revisar, refatorar ou escrever TS/JS e nomear um problema de design (método longo, muitos parâmetros, duplicação, acoplamento)                                                                                   | Detectar bug funcional (testes/type-checker), medir performance (profiling/benchmarks) ou aplicar estilo automático (ESLint/Prettier) |
| `code-standards-en`            | Nomear/renomear função, variável, classe ou arquivo; extrair magic number; separar parâmetro boolean-flag; revisar PR — inglês, verbo-first, CQS, early return, tamanho                                          | Política do produto exige identificadores localizados (não inglês)                                                                    |
| `context7`                     | Consultar documentação/API atualizada de qualquer lib, framework, SDK ou CLI antes de codar                                                                                                                      | Código interno deste repositório ou conteúdo não técnico                                                                              |
| `skill-best-practices`         | Criar ou revisar uma skill deste repositório: pastas `scripts/`/`references/`/`assets/`, metadados, `description`                                                                                                | Documentação geral, README ou código de lib que não seja uma skill                                                                    |
| `typescript-patterns`          | Criar/revisar `.ts`: nomear arquivo/tipo, modelar classe/interface/enum, valor default de campo, trocar loop por métodos de array                                                                                | Casing de identificador (`code-standards-en`), layout Angular (`angular-folder-structure`), DTOs HTTP (`angular-http`)                |
| `typescript-sonarqube`         | Qualquer `.ts`/`.tsx`/`.js`/`.jsx` criado, editado ou refatorado; antes de commit/PR; pedido de revisão "Sonar"/"SonarLint"                                                                                      | Formatação pura de código (Prettier/ESLint) sem relação com regras Sonar                                                              |
| `vitest-testing`               | Escrever/revisar teste Vitest (`.spec.ts`/`.test.ts`): mocks `vi`, AAA, timers, integração HTTP sem supertest                                                                                                    | Projeto usa Jest ou Sinon como stack principal de mock                                                                                |

**Nota transversal:** quando a tarefa depende de uma lib/framework/versão específica, consulte
`context7` (ou o MCP do Angular CLI, quando a skill Angular já indicar) antes de codificar —
`angular-folder-structure`, `angular-patterns`, `angular-http` e `typescript-patterns` já abrem
seu próprio fluxo recomendado com essa consulta. `typescript-sonarqube` roda a cada alteração de
`.ts`/`.tsx`/`.js`/`.jsx`, não só na revisão — funciona como o gate final de qualquer um dos
fluxos abaixo.

**Ordem sugerida por tarefa:**

- **Projeto/feature Angular nova** → `angular-folder-structure` (estrutura), depois
  `angular-developer` (arquitetura ampla), `angular-patterns`, `typescript-patterns`,
  `code-standards-en`, `code-smell`.
- **HTTP/REST em Angular** → `angular-http` (DTOs, mock, specs), depois `typescript-patterns`,
  `code-standards-en`, `code-smell`.
- **Componente/UI Angular sem HTTP** → `angular-patterns`, depois `typescript-patterns`,
  `code-standards-en`, `code-smell`.
- **TypeScript genérico fora do Angular** → `typescript-patterns`, depois `code-standards-en`,
  `code-smell`.
- **Revisão antes de commit ou PR** → `code-standards-en`, `code-smell`, `typescript-patterns`
  (conforme aplicável) — feche sempre com `typescript-sonarqube`.
- **Testes** → `vitest-testing` + a skill da camada testada (`angular-patterns` para componente,
  `angular-http` para serviço HTTP, `typescript-patterns` para domínio puro).
- **Nova skill de agente** → `skill-best-practices`.

# Persistência do Modo Plano

<plan_file>`.claude/plans/[timestamp]-[plan-slug].md`</plan_file>

- **OBRIGATÓRIO ABSOLUTO**: No modo Plano, após o usuário aceitar um plano, **SEMPRE** escreva o plano
  aceito em um arquivo Markdown dentro de <plan_file>.
- **OBRIGATÓRIO**: Se o plano aceito for atualizado posteriormente, atualize ou adicione o respectivo
  arquivo Markdown em <plan_file>.

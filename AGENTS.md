
# AGENTS.md

Guia para agentes de IA trabalhando neste projeto. As skills ficam em
`.agents/skills/` e são versionadas via `skills-lock.json`. Acione a skill
relevante **antes** de escrever ou revisar código na área coberta por ela.

## Resumo das skills

| Skill                        | O que faz                                                                                                                                                                                                                       | Quando usar                                                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **angular-developer**        | Gera código Angular e dá orientação de arquitetura (signals, `linkedSignal`, `resource`, forms, DI, routing, SSR, acessibilidade/ARIA, animações, estilização, testes, CLI).                                                    | Ao **criar ou alterar** componentes, serviços ou diretivas — signals, `resource`/`httpResource`, forms reativos, `inject()`, rotas lazy, SSR ou ARIA/foco. Sempre rode `ng build` ao final.                                   |
| **angular-material**         | Biblioteca de UI Angular Material (Material Design 3) e o CDK.                                                                                                                                                                  | Ao **adicionar ou configurar** componentes Material (button, form-field, select, paginator, card, dialog…), tema M3 ou primitivos do CDK. Confira a referência do componente antes de usar. Não misturar com outra lib de UI. |
| **code-smell**               | Catálogo de 94 code smells em TypeScript/JS em 12 categorias, com exemplo "antes/depois" e refatoração.                                                                                                                         | Ao **escrever ou revisar** código novo/refatoração: cite o smell pelo nome em inglês e justifique por SOLID/Clean Code. Não serve para detectar bugs funcionais.                                                              |
| **code-standards-en**        | Padrões de código: identificadores em inglês, casing, funções iniciadas por verbo, parâmetros como objeto, CQS, early returns, limites de tamanho de método/classe, comentários mínimos.                                        | Ao **nomear ou estruturar** funções/classes e revisar PRs: identificadores em inglês, verbo-first, params como objeto, CQS, early returns, limites de tamanho. Não usar quando a política exigir nomes localizados.           |
| **context7**                 | Recupera documentação técnica atualizada e exemplos de código de qualquer tecnologia via CLI `ctx7`.                                                                                                                            | Ao **consultar** docs/API de qualquer lib antes de codar: `ctx7 library <nome>` → `ctx7 docs <id>`. Máx. 3 chamadas; em quota, caia para os links oficiais e avise.                                                           |
| **folder-structure-angular** | Define/estrutura a árvore de pastas Angular: organização por feature sob `src/app` com camadas `core`/`shared`/`features`, standalone, arquivos co-localizados, rotas lazy, nomes com hífen.                                    | Ao **iniciar ou organizar** projeto/feature e decidir onde um arquivo vive (`core`/`shared`/`features`). Em projeto existente, espelhe a convenção. Para módulos HTTP, use `http-angular`.                                    |
| **folder-structure-backend** | Define a estrutura de pastas do backend em três camadas sob `backend/src/`: `controllers/` (HTTP), `services/` (regras de domínio) e `data/` (IO/persistência), com o fluxo obrigatório `HTTP → controllers → services → data`. | Ao **criar** módulos, rotas ou handlers de backend sob `backend/src/`, seguindo `HTTP → controllers → services → data`. Não usar para layout de frontend (veja `folder-structure-angular`).                                   |
| **http-angular**             | Cria módulos de integração HTTP padronizados sob `services/http`: serviços `HttpClient` tipados, DTOs com `@NoNull()` e `create()/createArray()`, mock service e testes.                                                        | Ao **criar ou padronizar** qualquer chamada REST: módulo em `services/http/http-<nome>/` com DTOs `@NoNull()` (`create()/createArray()`), mock service e specs. Não usar para lógica de UI.                                   |
| **vitest-testing**           | Orienta testes unitários e de integração com Vitest: mocks com `vi`, Arrange–Act–Assert, fake timers para `Date`, testes de endpoint HTTP sem supertest.                                                                        | Ao **escrever ou refatorar** testes: mocks com `vi`, Arrange–Act–Assert, um comportamento por teste, fake timers para `Date`, HTTP como integração. Não usar em projetos Jest/Sinon.                                          |

## Convenções rápidas

- **Stack:** Angular (frontend em `frontend/`), TypeScript, testes com Vitest.
- **Estrutura:** organizar por feature (`core`/`shared`/`features`), standalone-first, sem `NgModule`.
- **HTTP:** todo código de integração isolado em `services/http/`, com DTOs `@NoNull()`.
- **Qualidade:** código e identificadores em inglês; valide com `ng build` / `ng lint` / testes.
- **Docs:** consulte `context7` (`ctx7`) quando a precisão importar, em vez de confiar na memória.

## Skills declaradas

As skills instaladas e seus hashes estão em [`skills-lock.json`](./skills-lock.json).
Cada skill vive em `.agents/skills/<nome>/SKILL.md`, com material de apoio em `references/`.
---
name: typescript-sonarqube
description: 'Reviews TypeScript/JavaScript against SonarQube/SonarJS rules and runs SonarQube-style static analysis locally. Trigger as the mandatory final gate on EVERY task that creates, edits, refactors, writes, or organizes `.ts`/`.tsx`/`.js`/`.jsx` code — in new or existing projects — before reporting the change as done or opening a PR, not only when Sonar is mentioned. Also trigger when a "SonarQube", "Sonar", "SonarLint" or "SonarJS" review is requested, to check code smells/bugs/complexity/security the way SonarQube would, or to set up Sonar analysis. Run last in the closing chain, after code-standards-en and code-smell. Offers two scopes: validate the whole project, or only the new/changed code.'
---

# SonarQube for TypeScript

SonarQube analyzes TypeScript through its **SonarJS** analyzer, which uses the TypeScript
compiler's semantic model and type-checking for accurate, type-aware findings. The same rule
engine ships as the open-source ESLint plugin **`eslint-plugin-sonarjs`**, so the most valuable
rules run locally with no server. Use that for code review; use a real SonarQube/SonarCloud
server only when the user wants the dashboard, quality gate, security (taint) analysis, or
coverage tracking.

## When to run

Run this skill as the **mandatory final gate on every task that creates, edits, refactors, or
writes TypeScript/JavaScript** — in a new or existing project, not only when "Sonar" is mentioned.
As soon as `.ts`/`.tsx`/`.js`/`.jsx` files are created, edited, or refactored — and again
before a commit or PR — do a quick pass before reporting the change as done. It closes the
chain **after** `code-standards-en` and `code-smell` — run it last, once naming and design
smells are already addressed.

### Choose the scope first
Before reviewing, decide (or ask the user) which scope applies:

- **New / changed code only** (default for an ongoing change) — review just what this change
  touched. Get the file/line set from `git`:
  - Unstaged + staged vs `HEAD`: `git diff --name-only HEAD -- "*.ts" "*.tsx" "*.js" "*.jsx"`
  - Staged only: `git diff --name-only --cached -- "*.ts" "*.tsx" "*.js" "*.jsx"`
  - A branch vs its base: `git diff --name-only <base>...HEAD -- "*.ts" "*.tsx"`
  - For tooling (step 2): `npx eslint $(git diff --name-only HEAD -- "*.ts" "*.tsx")`
- **Whole project** — review/scan the entire codebase (full audit, onboarding, quality gate,
  or when the user asks "check the whole project"). For tooling, run ESLint / the scanner over
  the configured source globs, not a git diff.

If the user has not said which they want, default to **new / changed code only** and tell them
they can ask for a full-project scan.

This skill is split into reference files. **Load only what the task needs** — do not read all
of them up front.

| File | Load it when… |
|---|---|
| `references/rules-reference.md` | Doing a review — verified Bug / Code Smell rule keys with fixes |
| `references/security.md` | The user cares about vulnerabilities, security hotspots, or injection/XSS |
| `references/clean-code-taxonomy.md` | The user asks about severities, MQR vs Standard mode, or how Sonar grades issues |
| `references/tooling-setup.md` | Setting up `eslint-plugin-sonarjs`, the scanner, `sonar-project.properties`, or IDE binding |

## Workflow

### 1. Manual review (default — no credentials, offline)
Read the files in the chosen scope (the `git diff` set for *new/changed code*, or the target
source tree for *whole project*). Load `references/rules-reference.md` and check the code
against those rule keys. If the user mentioned security, also load `references/security.md`.
Report findings as:

```
file:line — Sxxxx (rule name) [Bug | Code Smell | Vulnerability | Security Hotspot]
  explanation + concrete fix
```

Group by issue type. For this repo specifically, watch the decorator wrappers in
`src/decorators/` for high **Cognitive Complexity (S3776)**, duplicated string literals
(**S1192**), and identical method bodies (**S4144**).

### 2. Run it locally (when the user wants tooling)
Load `references/tooling-setup.md` and wire up `eslint-plugin-sonarjs` into the existing flat
config (`eslint.config.ts`). Findings surface as `sonarjs/<rule>`.

### 3. Full server analysis (only if requested)
Load `references/tooling-setup.md` for `sonar-project.properties`, the scanner invocation,
`tsconfig` handling, and JetBrains Connected Mode. Server analysis is the **only** way to get
the deeper security (taint) rules from `references/security.md`.

## Hard rules
- **Never invent rule keys.** Every key you cite must come from a reference file here or be
  verified at https://rules.sonarsource.com/typescript/. If unsure, say so.
- Prefer steps 1–2 for day-to-day review; they need no token and run offline.
- Never commit a `SONAR_TOKEN` — pass it via the environment.

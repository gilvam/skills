---
name: symlinks
description: Skill para criar, reparar ou verificar links simbólicos de .agents/skills para pastas de skills de outros agentes locais, especialmente .claude/skills e .devin/skills. Use quando o usuário pedir para sincronizar, linkar, espelhar, compartilhar, "colar" ou expor as mesmas skills em múltiplas ferramentas de IA, independentemente do sistema operacional.
---

# Symlinks — Links de Skills entre Agentes

Crie links simbólicos para que múltiplas ferramentas de IA leiam a mesma pasta canônica de skills.

Origem canônica:

```text
.agents/skills
```

Destinos padrão:

```text
.claude/skills
.devin/skills
```

## Quando aplicar esta skill

Use esta skill quando o usuário pedir para:

- Criar symlinks de `.agents/skills` para outras pastas de agentes.
- Sincronizar skills entre Codex, Claude, Devin ou outra ferramenta local.
- Fazer `.claude/skills` ou `.devin/skills` apontarem para `.agents/skills`.
- Verificar se os links já existem e apontam para o lugar certo.
- Reparar links quebrados sem copiar as skills.

Não use esta skill para copiar arquivos entre pastas. A intenção é manter uma única fonte de verdade em `.agents/skills`.

## Fluxo recomendado

1. **Identifique a raiz do projeto**: use o diretório atual, a menos que o usuário indique outro caminho.
2. **Valide a origem**: confirme que `.agents/skills` existe e é um diretório.
3. **Inspecione os destinos**: verifique `.claude/skills` e `.devin/skills` antes de alterar qualquer coisa.
4. **Crie os pais ausentes**: crie `.claude` e `.devin` quando necessário.
5. **Crie os links**: faça cada destino apontar para `.agents/skills` usando o mecanismo nativo do sistema operacional.
6. **Preserve o que já está correto**: se o destino já é um link para `.agents/skills`, não altere.
7. **Pare em conflitos**: se o destino existe como arquivo, diretório real ou link para outro lugar, reporte o conflito e peça confirmação antes de substituir.
8. **Verifique o resultado**: confirme que cada destino resolve para o mesmo diretório da origem.

## Comandos por ambiente

Escolha os comandos de acordo com o ambiente onde a IA está executando.

### Windows PowerShell

Use `New-Item -ItemType SymbolicLink` para links simbólicos de diretório:

```powershell
New-Item -ItemType SymbolicLink -Path ".claude/skills" -Target ".agents/skills"
New-Item -ItemType SymbolicLink -Path ".devin/skills" -Target ".agents/skills"
```

Se o Windows negar a criação de symlink por falta de Developer Mode ou permissão elevada, pergunte ao usuário se pode usar junction:

```powershell
New-Item -ItemType Junction -Path ".claude/skills" -Target ".agents/skills"
```

### macOS, Linux e POSIX

Use `ln -s`, preferindo caminho relativo estável:

```bash
mkdir -p .claude .devin
ln -s ../.agents/skills .claude/skills
ln -s ../.agents/skills .devin/skills
```

Antes de executar `ln -s`, confirme que cada destino ainda não existe.

## Validação

Depois de criar os links:

- No PowerShell, use `Get-Item`, `Resolve-Path` e a propriedade `Target`.
- Em POSIX, use `ls -l`, `readlink` ou `realpath`.
- Reporte ao usuário quais destinos foram criados, quais já estavam corretos e quais ficaram bloqueados por conflito.

## Regras de segurança

- Não copie o conteúdo de `.agents/skills`.
- Não remova nem substitua destinos existentes sem aprovação explícita.
- Não transforme `.agents/skills` em link para outra pasta; ela é a origem canônica.
- Prefira links relativos quando eles continuarem válidos dentro do layout do projeto.
- Trate symlink e junction como equivalentes apenas quando o objetivo for expor a mesma pasta local para outra ferramenta.

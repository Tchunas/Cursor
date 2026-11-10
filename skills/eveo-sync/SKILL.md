---
name: eveo-sync
description: >-
  Sincronizar skills EVEO entre EveoNetwork/skills-do-time, o espelho
  Tchunas/Cursor e o Agent Store pessoal. Usar quando o usuário pedir sync,
  espelho, atualizar skills EVEO ou alinhar repositórios.
---

# Sync EVEO ↔ Tchunas/Cursor

## Fonte da verdade

Trabalho EVEO usa a org [EveoNetwork](https://github.com/orgs/EveoNetwork/repositories):

| Repo | Escopo |
|------|--------|
| `EveoNetwork/skills-do-time` | Skills compartilhadas |
| `EveoNetwork/padroes-e-documentacao` | Padrões, catálogo, política de IA, protocolo de sync |
| `EveoNetwork/automacoes-infra` | Automações de infraestrutura |

Espelho pessoal: `Tchunas/Cursor` + Agent Store (`skills/`).

## Direção

1. **EVEO → pessoal**: espelhar `skills/` (e metadados úteis) de `skills-do-time` para `Tchunas/Cursor` e para o Agent Store.
2. **Pessoal → EVEO**: NÃO espelhar automaticamente. Seguir Issue + PR e `prompts/SYNC.md` em `padroes-e-documentacao`.

## Procedimento de espelho (EVEO → pessoal)

1. Autenticar com `gh` já logado (nunca gravar PAT do chat).
2. Obter SHA de `EveoNetwork/skills-do-time` em `main`.
3. Baixar zipball/tarball e extrair.
4. Copiar `skills/*` para o Agent Store pessoal.
5. Publicar os mesmos arquivos em `Tchunas/Cursor` (pasta `skills/` + `AGENTS.md`, `catalogo.json`, etc.).
6. Atualizar `SYNC.md` no espelho com o SHA sincronizado.
7. Relatar: SHA, skills sincronizadas, pendências.

## Regras

- Ler `AGENTS.md` / `CONTRIBUTING.md` antes de alterar conteúdo EVEO.
- Não publicar credenciais, dados de cliente ou caminhos pessoais.
- Não enfraquecer governança nem fazer merge automático em `main` dos repos EVEO.

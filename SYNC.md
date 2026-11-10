# Sync EVEO <-> Tchunas/Cursor

## Fonte da verdade (trabalho EVEO)
- Org: https://github.com/orgs/EveoNetwork/repositories
- Skills: EveoNetwork/skills-do-time
- Padroes: EveoNetwork/padroes-e-documentacao
- Automacoes: EveoNetwork/automacoes-infra

## Espelho pessoal
- Tchunas/Cursor — skills EVEO espelhadas + skills pessoais de aprendizado
- Agent Store local alinhado

## Ultimo sync
- Origem EVEO skills-do-time: `033ce3a8c293490a55bee2d4363cb8aa6290b127`
- Skill pessoal nova: `sp2-bgp-transit` (consulta trânsitos BGP SP2, 2026-11-10)
- Direcao EVEO -> pessoal para skills oficiais; skills pessoais sobem so no espelho Tchunas/Cursor ate PR EVEO

## Manter sincronizado
1. Pedir sync / usar skill `eveo-sync`
2. Espelhar `skills/` EVEO para este repo e Agent Store
3. Atualizar este arquivo
4. Nunca publicar tokens/credenciais
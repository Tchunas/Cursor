---
name: sp2-bgp-transit
description: >-
  Listar e validar sessões BGP de trânsito de internet nos roteadores de borda
  SP2 (SP2-DH3-F-RT-1/2, Junos MX204). Usar quando o usuário pedir trânsitos,
  upstreams ou sessões BGP de internet em SP2.
---

# SP2 — sessões BGP com trânsitos de internet

Complementa [networkai-bgp](../networkai-bgp/SKILL.md). Escopo: consulta **somente leitura** nos RTs de SP2 para identificar trânsitos dedicados (não IX, não cliente, não Anti-DDoS).

## Alvos conhecidos

| Hostname | IP gestão | Plataforma | ASN local |
|----------|-----------|------------|-----------|
| SP2-DH3-F-RT-1 | 10.2.0.3 | MX204 / Junos | 53107 |
| SP2-DH3-F-RT-2 | 10.2.0.142 | MX204 / Junos | 53107 |

Autenticação: reutilizar sessão/`gh`/credencial já válida do operador. **Nunca** gravar senha/PAT em skill, repo ou chat de resposta. Se credencial for colada no chat, avisar para rotacionar.

## Como distinguir trânsito

Trânsito dedicado em SP2 costuma aparecer como **grupo BGP externo** com nome de provedor e tabela ampla (~full table). Não classificar automaticamente como trânsito:

- grupos `IX.BR_PEERS*` (peering de IX, ex. Hurricane Electric AS6939)
- clientes / peers nomeados (`*_PEERS`, Metaweb, etc.)
- `iBGP-RTs*`, `COREs-*`, `WANGUARD`
- `ROCKET` / `GAMERSCLUB` (Anti-DDoS / especial; pode receber tabela cheia)

Grupos de trânsito observados nesta topologia:

| Grupo | AS peer | Presente em |
|-------|---------|-------------|
| `ASCENTY_V4` / `ASCENTY_V6` | 52925 | RT-1 e RT-2 |
| `ALGAR_V4` / `ALGAR_V6` | 16735 | RT-1 |
| `UPX_V4` / `UPX_V6` | 52863 | RT-1 e RT-2 |
| `UPIX_v4` / `UPIX_v6` | 40850 | RT-2 (descrição `#IP-TRANSITO_UPIX_10G` no v6) |

Neighbors mudam por RT (ex. Ascenty IPv4: `.1` no RT-1, `.5` no RT-2). Sempre confirmar na config/operacional atual.

## Coleta mínima (Junos)

1. Alcance SSH (TCP/22). Plataforma: `show version \| no-more`.
2. ASN local: `show configuration routing-options autonomous-system \| display set`.
3. Inventário de grupos:  
   `show configuration protocols bgp \| display set \| match "group |type |peer-as |neighbor |description"`  
   Redigir `authentication-key*` se aparecer.
4. Para cada grupo candidato a trânsito:  
   `show configuration protocols bgp group <NOME> \| display set`  
   `show bgp group <NOME>`
5. Conferir estado Established e contadores `Active/Received/Accepted/Damped` por família.
6. Observar export peculiar: UPX usa `export [ UPX-OUT BGP-DENY-ALL ]` (recebe; anúncio efetivo pode estar negado). UPIX IPv4 pode estar Established com `0` ativos/aceitos — reportar como anomalia de policy, não como “sem sessão”.

Não executar clear/refresh, commit, debug ou mudanças. Soft clear também é operação ativa.

## Formato da resposta

Por roteador, tabela: Trânsito | AS | Família | Neighbor | Estado | Act/Rec/Acc. Em seguida notas (deny-all, 0 aceitos, grupo ausente no peer RT). Separar explicitamente o que **não** é trânsito se o usuário misturar IX/cliente.

## Baseline de referência (consulta 2026-11-10)

Evidência operacional pontual; revalidar na próxima tarefa.

**RT-1:** Ascenty v4/v6, Algar v4/v6, UPX v4/v6 — Established; sem UPIX.

**RT-2:** Ascenty v4/v6, UPX v4/v6, UPIX v4/v6 — Established; sem Algar; UPIX IPv4 com ~1M recebidos e 0 ativos/aceitos.

## Critério de aceite

Dado acesso autorizado aos dois RTs, a skill deve produzir a lista de sessões dos grupos de trânsito com peer/AS/família/estado/contadores, sem credenciais e sem confundir IX.br ou Anti-DDoS com trânsito.

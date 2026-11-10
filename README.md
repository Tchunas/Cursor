# skills-do-time
Skills de IA compartilhadas da EVEO, com exemplos e critérios de validação.


## Comece aqui

Leia [AGENTS.md](AGENTS.md), [CONTRIBUTING.md](CONTRIBUTING.md), o [catálogo oficial](https://github.com/EveoNetwork/padroes-e-documentacao/blob/main/CATALOGO.md) e a [política de IA](https://github.com/EveoNetwork/padroes-e-documentacao/blob/main/POLITICA-IA.md).

O repositório disponibiliza sete skills experimentais: roteamento IP, BGP, IGP, policies [Nettools SP3](skills/networkai-nettools-sp3/SKILL.md), [fluxos FortiGate](skills/networkai-fortigate-flow/SKILL.md) e [acesso de contingência](skills/networkai-contingency-access/SKILL.md). Consulte [catalogo.json](catalogo.json), os exemplos de aceite nos SKILL.md e o [exemplo Nettools](examples/nettools-sp3/README.md). Integração não equivale a promoção active, instalação automática ou deploy. Veja [publicação durante tarefas ativas](PUBLICACAO.md).

Valide com python3 scripts/check_governance.py. A rotina Governanca EVEO roda em main e PRs. O check não é obrigatório no servidor enquanto o plano/permissões não suportarem proteção.

O [guia de firewall e contingência](docs/FIREWALL_CONTINGENCY.md) explica os exemplos, a procedência e os limites de validação.

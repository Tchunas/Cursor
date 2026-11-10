# Fontes e procedência

Revisão em 23/10/2026. Procedimento redigido localmente; nenhum script ou pacote
externo foi instalado.

- [OSPFv2 — RFC 2328](https://www.rfc-editor.org/rfc/rfc2328.html): FSM, eleição, LSAs e cálculo.
- [OSPFv3 — RFC 5340](https://www.rfc-editor.org/rfc/rfc5340.html): transporte e diferenças de LSAs.
- [Integrated IS-IS — RFC 1195](https://www.rfc-editor.org/rfc/rfc1195.html): níveis e cálculo IP.
- [Multi-topology IS-IS — RFC 5120](https://www.rfc-editor.org/rfc/rfc5120.html): topologias por família e anúncio.
- [EIGRP — RFC 7868](https://www.rfc-editor.org/rfc/rfc7868.html): DUAL e estado de rotas; publicação Informational.
- [Juniper: troubleshooting MPLS/IGP](https://www.juniper.net/documentation/us/en/software/junos/mpls/topics/topic-map/troubleshooting-mpls.html): comandos e verificação de adjacências.

A [skill OSPF do aiNOC](https://github.com/pdudotdev/aiNOC/blob/733cc1480a8baef96517e60a2c6849a5ffbd592c/skills/ospf/SKILL.md),
blob a8cacfca75e2585ce9e1284c85d8507b8e4eff85, foi avaliada como referência
conceitual, sem copiar texto/código GPL. A organização por sintoma é útil;
não foram adotados dependência de ferramentas próprias, reset em Loading,
retorno obrigatório a timers padrão nem tabelas simplificadas de NSSA.
Confirmar extensões e CLI na documentação da release antes de aplicação real.


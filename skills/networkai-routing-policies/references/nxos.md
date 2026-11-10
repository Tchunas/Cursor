# Avaliação de route-map e prefix-list NX-OS

Descobrir aplicação, direção e address family antes de interpretar uma route-map.
Uma route-map usada em export BGP, redistribuição ou policy-based routing não tem
necessariamente o mesmo efeito de default sobre uma rota ou um pacote.

Seguir as sequências efetivas da route-map e os objetos de match referenciados.
Um `deny` numa prefix-list usada como match pode apenas fazer aquela cláusula
não casar; isso não equivale automaticamente a negar a rota na route-map.
Examinar as próximas sequências e o default do contexto de aplicação.

Comparar prefixo e comprimento com `ge`/`le`, rede canônica, família e semântica
de cada match. Confirmar suporte a chamadas/continuidade na release utilizada.
Não transferir a semântica de termos Junos para sequências NX-OS.

Ao acrescentar ou remover community, conferir se os atributos anteriores são
preservados e se outras rotas usam o mesmo objeto. Confirmar o resultado por
rota e peer, com atenção às variantes pós-processamento de advertised-routes.
Não habilitar recursos nem limpar sessões para facilitar a validação.

Fonte: [políticas BGP Nexus 9000, NX-OS 9.2(x)](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/92x/unicast/configuration/guide/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-92x/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-92x_chapter_01010.html).

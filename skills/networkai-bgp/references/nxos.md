# BGP em NX-OS

Identificar família Nexus, release, VRF e address family. Não aplicar sintaxe de
Nexus 9000 a Nexus 5000 sem confirmar suporte. Templates de peer, address family
e herança podem mudar as policies efetivas.

Em releases Nexus 9000 compatíveis, os pontos de partida incluem:

```text
show bgp ipv4 unicast summary
show bgp ipv4 unicast neighbors PEER
show bgp ipv4 unicast neighbors PEER advertised-routes
show ip route PREFIXO
```

Conferir a posição de `vrf` e a sintaxe IPv6 na ajuda da release. Consultar a
configuração do BGP, route-map e prefix-list específicos. Distinguir rota que
consta na tabela BGP de best path e da instalada na RIB/FIB.

Algumas releases oferecem `advertised-routes processed` para visualizar atributos
após route-map de saída; confirmar disponibilidade e semântica antes de concluir
que a saída comum contém todos os atributos reescritos. Received-routes pode
depender de retenção de rotas; sua ausência não autoriza soft clear ou habilitar
soft-reconfiguration automaticamente.

Para sessão que não estabelece, usar os endereços local/remoto e estado do
detalhe do peer; conferir update-source, rota/retorno na VRF, multihop/GTSM e
último reset. Recortar logs e contadores de controle conforme a hipótese, sem
habilitar debug. Um ping pela VRF management não valida uma sessão em outra VRF.
Para instabilidade, ler a referência específica antes de interpretar filas,
limite de prefixos ou sugerir qualquer clear.

Fonte: [BGP Nexus 9000, NX-OS 9.2(x)](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/92x/unicast/configuration/guide/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-92x/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-92x_chapter_01010.html).

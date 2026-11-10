# IS-IS e EIGRP

## IS-IS

Identificar instância, interfaces, NET/system ID, níveis L1/L2, circuit type e
topologia IPv4/IPv6. L1 exige área compatível; uma adjacência L2 pode ligar áreas
diferentes. System ID deve ser único. Área diferente não é motivo universal para
declarar falha. IS-IS roda diretamente sobre enlace/CLNS; não testar o protocolo
como se dependesse de uma porta TCP ou de ping IPv4.

Percorrer IIH -> adjacency -> LSP/CSNP/PSNP -> SPF por nível/topologia -> RIB/FIB.
Comparar autenticação por presença, padding/MTU, timers/holdtime, overload,
métrica narrow/wide e famílias. Overload ou attached bit podem alterar trânsito
e default sem derrubar vizinhos. Em LAN há DIS/pseudonode; DIS pode mudar por
preempção, diferentemente do DR OSPF. Em ponto a ponto não exigir eleição DIS.

Uma adjacência IPv4 Up não comprova prefixos IPv6 no mesmo cálculo. Conferir
multi-topology versus single-topology e anúncios realmente presentes. SR SIDs
exigem ainda interpretação de labels/SRGB, não apenas IGP Up. Se a evidência
necessária de MPLS não estiver no escopo, registrar a lacuna sem concluir que
IGP Up comprova transporte por labels.

## EIGRP

Confirmar suporte efetivo da plataforma, modo clássico/named, AF/VRF, AS,
participação da interface, K-values, autenticação, passive e conectividade.
RFC 7868 descreve EIGRP de forma Informational; isso não implica implementação
em todo fabricante. Não recomendar EIGRP em Junos sem suporte documentado.

Separar neighbor, topology table, successor/feasible successor e RIB/FIB.
Passive na topology table significa rota estável; passive-interface significa
ausência de formação de vizinhança naquela interface. Não confundir os dois.
A condição de viabilidade usa reported distance menor que feasible distance;
uma alternativa que não a satisfaz não é automaticamente inválida, mas pode
exigir cálculo difusor antes de substituição. Active/Queries/SIA pedem evidência
de resposta, alcance de queries, stub, perda e recursos; não resolver com clear
global nem "no auto-summary" aplicado sem contexto de versão/desenho.

Em redistribuição, verificar seed metric/metric vector, external/internal,
route tags e realimentação. Não comparar números de métricas EIGRP e OSPF
como se representassem o mesmo custo. Documentar diferenças de suporte,
convergência e política antes de uma migração de protocolo.


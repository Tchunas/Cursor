# Coleta de IGP

Exemplos de leitura: substituir parâmetros e conferir ajuda/documentação do OS
real. VRF/processo/AF e posição dos argumentos variam; não executar placeholders
nem presumir que um comando existe em IOS, NX-OS e IOS-XR com a mesma sintaxe.
Consultar prefixos/interfaces específicos, evitando dump de LSDB inteira em
equipamento com carga. Não incluir trechos de autenticação na saída compartilhada.

| Objetivo | IOS/IOS-XE | Junos |
| --- | --- | --- |
| Estado e parâmetros OSPF | show ip ospf neighbor; show ip ospf interface INTERFACE | show ospf neighbor detail; show ospf interface detail |
| OSPFv3 IPv6 | show ipv6 ospf neighbor quando esse modo for usado; confirmar modo ospfv3/AF | show ospf3 neighbor; show ospf3 interface |
| LSDB | show ip ospf database, limitado ao tipo/prefixo necessário | show ospf database detail, limitado ao alvo |
| IS-IS | show isis neighbors; show isis database | show isis adjacency detail; show isis database |
| EIGRP | show ip eigrp neighbors; show ip eigrp topology | Não assumir implementação |
| Instalação | show ip route PREFIXO; FIB específico da plataforma | show route table TABELA PREFIXO extensive; show route forwarding-table destination IP |

Preferir saídas estruturadas e parsers existentes com cobertura conhecida.
CLI error, falta de privilégio, resultado vazio e estado Down são coisas
diferentes. Anotar timestamp e intervalo; não executar ping, captura ou debug
só porque uma tabela veio vazia.


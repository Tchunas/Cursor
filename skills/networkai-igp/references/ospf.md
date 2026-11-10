# OSPF: interpretar antes de corrigir

Confirmar versão, instância, interface, area ID, network type, RID e família.
Process ID no IOS é local: números diferentes entre roteadores não impedem
adjacência. OSPFv2 usa protocolo IP 89; não é TCP/UDP 89. OSPFv3 usa IPv6/link-local
no caso IPv6 e também admite outras famílias por extensões; confirmar AF,
instance ID, transporte e proteção realmente usados.

## Adjacência

| Evidência | Interpretação e próxima verificação |
| --- | --- |
| Down/ausente | Admin/oper, participação/passive, Hellos, VLAN/MTU de caminho, ACL/CoPP e parâmetros; ausência da linha não identifica causa |
| Init | Hello recebido sem reconhecimento bidirecional do RID local; verificar o caminho inverso e os parâmetros, sem presumir autenticação pelo estado |
| 2-Way em broadcast | Normal entre DROthers; conferir DR/BDR, prioridade e adjacências que deveriam chegar a Full |
| ExStart/Exchange | Comparar MTU anunciada/aceita em DBD, RID único, retransmissões, perda e network type; não provar MTU só pelo estado |
| Loading | LS Requests/retransmissões pendentes; identificar LSA e entrega antes de sugerir reset |
| Full | Base sincronizada com esse vizinho; não comprova rota selecionada nem tráfego fim a fim |

Comparar Hello/dead efetivos, área e opções stub/NSSA, autenticação por presença,
algoritmo/key-id e validade temporal; nunca coletar chave em claro. Timers
customizados compatíveis podem ser intencionais. Não usar 10/40 como exigência
universal; BFD e dead interval medem mecanismos diferentes.

Network type não é um simples campo negociado igual ao Hello timer: algumas
divergências permitem Full e geram cálculo de topologia incorreto. Verificar
a representação da ligação nos Router/Network LSAs. DR não preemptivo e prioridade
zero não são falhas por si. Não usar mtu-ignore como conserto padrão; separar
checagem de DBD da capacidade real de transportar pacotes.

## Prefixo na LSDB, ausente da RIB

Para OSPFv2, identificar tipo e advertising router: Type 1/2 locais à área,
Type 3/4 de ABR, Type 5 externas, Type 7 dentro da NSSA. Não transportar essa
numeração literalmente para OSPFv3; usar tipos/escopos e prefix LSAs próprios.
Uma NSSA não aceita Type 5 de fora e pode originar Type 7; Type 7 não é inundada
como Type 7 no backbone. Conferir elegibilidade e estado da tradução no ABR,
P-bit, alcance do forwarding address e política; maior RID não resume toda
eleição/forçamento de translator. No-summary não elimina toda Type 3: a default
pode continuar presente.

Checar validade/idade da LSA, SPF, alcance do ASBR/forwarding address, filtros
de instalação, rota concorrente, VRF e endereço exato. Filtrar instalação na RIB
não equivale a remover a LSA da inundação. Sumarização pode explicar ausência de
mais específicos. Área sem adjacência de backbone válida requer revisão do
desenho, sem criar virtual-link automaticamente.

Em externas elegíveis para o mesmo prefixo, E1 incorpora custo interno; E2
prioriza métrica externa e pode usar custo interno no desempate. Elegibilidade,
intra/inter-area e regras de seleção precedem simplificações como "menor número".
Não alterar tipo/métrica para ganhar uma comparação entre rotas que nem são
alcançáveis. Registrar o motivo real de instalação/recusa e consultar RFC/release
para casos NSSA, múltiplos ABRs ou extensões.


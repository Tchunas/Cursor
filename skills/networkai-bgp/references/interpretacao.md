# Decisões de diagnóstico BGP

| Evidência | Conclusão permitida | Próximo recorte útil |
| --- | --- | --- |
| Peer Established, contagem de anúncios positiva | Existe sessão e algum anúncio | Prefixo exato na Adj-RIB-Out/pós-export da família pretendida |
| Duas rotas, só a inativa contém a community exigida | Os atributos não podem ser combinados entre caminhos | Motivo de inatividade, origem e atributos completos da ativa |
| Export permite a rota no papel | Resultado da avaliação configurada | Elegibilidade BGP, next hop e anúncio efetivo ao peer |
| Rota exportada localmente | O roteador a preparou para esse vizinho | Confirmação no vizinho/coletor, se autorizados |
| Agregado discard e mais específico unicast | O agregado não determina sozinho o destino do host | FIB por IP de destino e resolução do próximo salto |
| Rota iBGP selecionada sem anúncio a outro iBGP | Pode haver regra de retransmissão, além da policy | Topologia de sessões, RR/clientes, originator/cluster e AF |

Em um mesmo prefixo, comparar preferência administrativa entre protocolos antes
dos critérios BGP aplicáveis. Não transportar números de preferência entre
fabricantes. Verificar opções que mudem a seleção/exportação, como add-path,
advertise-inactive e reflexão; não sugerir habilitá-las para contornar o problema.

Se um controlador injeta uma rota mais preferida, verificar sua identidade e
import policy. Não inventar uma estática discard para substituir a rota legítima
nem cortar uma sessão inteira para corrigir um único prefixo.

Fonte: [políticas BGP Junos](https://www.juniper.net/documentation/us/en/software/junos/bgp/topics/topic-map/basic-routing-policies.html).

## Estado atual e histórico de erros

Last Error, mensagens Cease e contadores Sent/Recv em show bgp neighbor podem
descrever eventos antigos com a sessão atual Established. Cruzar estado, uptime,
última transição e horário do incidente antes de concluir queda ou defeito.
Um parser que procura a palavra Error em qualquer linha pode classificar uma
resposta válida como erro de CLI; preservar a saída e distinguir os dois casos.

# BGP em IOS e IOS-XE

Confirmar modelo/release, VRF, AF e peer antes de compor os comandos. IOS, IOS-XE,
IOS-XR e NX-OS têm sintaxe e comportamentos diferentes. Os exemplos abaixo são
pontos de partida para IPv4 unicast global em IOS-XE compatível; PEER e PREFIXO
são parâmetros, não texto pronto para colar. Resolver sintaxe de VRF/IPv6 na ajuda
ou documentação da release, mantendo a mesma origem/tabela nas verificações.

```text
show bgp ipv4 unicast summary
show bgp ipv4 unicast neighbors PEER
show bgp ipv4 unicast PREFIXO
show bgp ipv4 unicast neighbors PEER advertised-routes
show ip route PEER
show ip route PREFIXO
```

Algumas versões oferecem formas equivalentes em `show ip bgp`. Não interpretar
erro de sintaxe, família incorreta ou output truncado como tabela vazia. Um número
em State/PfxRcd pode indicar sessão estabelecida com essa contagem; Idle/Active
não são contagem zero. Linhas IPv6 podem quebrar, ASNs podem ter quatro bytes e
o layout depende da release: reutilizar parsers existentes e validar o endereço.

## Transporte e eventos

No detalhe do peer, ler endereço local, estado, uptime, último reset, erro e timers
negociados. Se a hipótese for TCP, consultar apenas sockets do peer por comando
`show tcp` suportado; se for queda, recortar logs pelo peer e período. Não obter
configuração inteira de BGP ou filtrar linhas contendo password/MD5 para imprimir
segredos. Selecionar objetos necessários e usar a redação do coletor existente.

Para alcance, um ping curto só ajuda se origem e VRF forem as da sessão; resposta
ICMP não prova TCP/179 liberado. Conferir retorno e proteção do plano de controle.
Os defaults comuns documentados no IOS-XE são keepalive 60 s e hold 180 s, mas
usar os valores efetivos do alvo, não assumir que todo fabricante usa 30/90.

## Rotas, policies e expressões AS-path

Detalhar o prefixo para separar caminhos BGP e motivo de seleção. Examinar apenas
as route-maps/prefix-lists efetivas do neighbor/AF. Uma entrada deny da prefix-list
usada como match não é, sozinha, o resultado final da route-map; usar a skill de
policies quando houver encadeamento, continue ou herança.

`received-routes` pode exigir retenção adicional. Se indisponível, registrar a
lacuna e usar evidência já existente dos dois lados autorizados. Não habilitar
soft-reconfiguration nem emitir route refresh/soft clear para preencher a lacuna.
Validar o que a saída advertised-routes mostra de atributos pós-policy nessa
release; não equiparar anúncio local a aceitação pelo provedor.

Para AS_SEQUENCE simples na sintaxe Cisco, diferenciar:

| Expressão | O que seleciona no campo AS-path |
| --- | --- |
| `_65001_` | Token AS 65001 em alguma posição; não casa 165001. |
| `^65001$` | Caminho inteiro formado somente pelo token 65001. |
| `_65001$` | Caminho que termina no token 65001, no caso AS_SEQUENCE simples. |
| `^$` | Caminho vazio; não seleciona todas as rotas iBGP. |

AS_SET, confederações, notação asdot e reescritas exigem interpretação própria.
Não inferir autorização de origem a partir de uma regex; conferir NLRI, caminho
e evidência de origem pertinente. Uma string numérica sem delimitadores pode
casar outros ASNs. Nunca usar o resultado de uma regex como prova de forwarding.

Fontes: [BGP e timers IOS-XE](https://www.cisco.com/c/en/us/td/docs/routers/ios-xe/ip-routing/b-ip-routing/m_irg-int-features-0.html),
[expressões regulares Cisco IOS](https://www.cisco.com/c/en/us/td/docs/ios/termserv/configuration/guide/xe_16/tsv-xe-book/tsv_reg_express.html),
[guia BGP Catalyst 9000](https://www.cisco.com/c/en/us/td/docs/switches/lan/c9000/lyr3-fwd/bgp/bgp-configuration-guide/routing-bgp.html).

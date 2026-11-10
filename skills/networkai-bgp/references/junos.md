# BGP em Junos

Confirmar release e contexto antes de montar comandos. Selecionar apenas os
objetos autorizados; os nomes em maiúsculas abaixo são parâmetros a resolver.
Para herança e inativos, conferir a configuração efetiva e distinguir candidato,
committed e estado operacional. Registrar a visão e o horário de cada evidência.

Consultas típicas para inet.0, ajustando a tabela quando houver instância:

```text
show bgp neighbor PEER
show route table TABELA PREFIXO exact extensive
show route advertising-protocol bgp PEER PREFIXO detail
show route forwarding-table destination IP_DESTINO
```

Consultar import/export no neighbor e grupo e as policies referenciadas. Não
inferir precedência de uma community pelo nome. A rota ativa determina o caminho
normal de exportação, sujeito às regras BGP e opções da plataforma. Comparar
atributos da rota ativa com o match da policy, mantendo os caminhos separados.

Uma rota anunciada para o grupo não identifica automaticamente todos os peers
que receberam o prefixo; conferir o vizinho exato. Comandos para recebidas,
hidden e tabela/instância variam: consultar a ajuda da release e não realizar
refresh de sessão para obter uma coleta que não foi previamente retida.

Para transporte, ler no detalhe do peer os endereços Local/Peer, estado, último
evento/erro, famílias negociadas, flags de limite e timers. Correlacionar logs
do período: `Last Error` pode ser histórico enquanto a sessão atual está saudável.
`BFD: disabled, down` não significa que uma sessão BGP Established está caída.

Usar `show route receive-protocol bgp` e `show route advertising-protocol bgp`
com peer e tabela/prefixo pertinentes, conforme suporte. Não transportar os
sufixos Cisco `received-routes`/`advertised-routes` para `show bgp neighbor`.
Summary Junos pode mostrar Active/Received/Accepted/Damped para cada RIB;
preservar o cabeçalho e as linhas da família antes de interpretar um zero.

Não generalizar que o Junos exige export explícito para toda rota BGP: conferir
os defaults, regras iBGP/RR e exceções da plataforma, inclusive PTX. Policy
explícita, candidato não commitado e configuração ativa são evidências diferentes.

Fontes: [consulta de anúncios](https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/command/show-route-advertising-protocol.html),
[políticas BGP](https://www.juniper.net/documentation/us/en/software/junos/bgp/topics/topic-map/basic-routing-policies.html),
[detalhe do vizinho](https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/command/show-bgp-neighbor.html),
[políticas padrão](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-routing-policies-actions-defaults.html).

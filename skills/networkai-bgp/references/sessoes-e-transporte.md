# Sessões BGP e transporte

Fixar peer, família de transporte, AFI/SAFI negociada, VRF, endereços local/remoto,
ASNs efetivos e horário. O transporte pode ser IPv6 e carregar NLRI IPv4 ou EVPN;
não escolher a tabela de rotas somente pela forma do endereço do peer.

## Estado orienta a próxima evidência

| Estado observado | Recorte que ajuda a distinguir causas |
| --- | --- |
| Idle | Shutdown administrativo, configuração efetiva, backoff, limite de prefixos e último evento. Não atribuir automaticamente à ACL. |
| Connect / Active | Tentativa de transporte TCP: rota na VRF correta, origem usada, retorno, listener e contadores de filtros. Active não significa sessão estabelecida. |
| OpenSent | Houve transporte suficiente para enviar OPEN; ler erro/NOTIFICATION, ASN, parâmetros e capacidades. Um snapshot não comprova estabilidade do TCP. |
| OpenConfirm | OPEN progrediu; conferir KEEPALIVE, NOTIFICATION, timers negociados e eventos subsequentes. |
| Established | Conferir AF negociada e as etapas recebido/aceito/selecionado/anunciado; sessão não prova entrega do tráfego. |

Uma causa pode levar a vários estados em momentos diferentes. Correlacionar
uptime e timestamp do erro; um erro antigo não explica sozinho uma sessão hoje
Established. Não repetir tentativas rápidas para observar cada transição.

## Origem e caminho

1. Identificar o endereço local real da sessão e o update-source/local-address
   efetivo. Ping feito com outra origem ou na tabela global não testa esse caminho.
2. Conferir rota até o peer na VRF correta e, quando houver evidência autorizada
   do outro lado, retorno até a origem local. Loopbacks precisam de alcance mútuo;
   sua utilização não é obrigatória em todo tipo de sessão.
3. Separar ping de TCP/179: ICMP respondendo não demonstra handshake TCP. A conexão
   usa uma ponta em 179 e a outra normalmente em porta efêmera; avaliar ambos os
   sentidos e a semântica stateful/stateless, sem exigir porta 179 nas duas pontas.
4. Para eBGP não diretamente conectado, conferir multihop, TTL e GTSM existentes
   nos dois lados. Não alterar esses mecanismos para experimentar.
5. Correlacionar filtros de interface, firewall intermediário e proteção do plano
   de controle com caminho, contadores e horário; CoPP não é sinônimo de ACL de
   trânsito. Usar a skill de ACL quando for necessário avaliar o fluxo detalhado.

## Negociação e autenticação

Ler código/subcódigo e direção da NOTIFICATION: Bad Peer AS, por exemplo, aponta
para o ASN esperado no OPEN. Conferir local-as, confederação e herança antes de
propor correção. Timeout sem resposta não comprova erro de autenticação.

Verificar presença/tipo e estado de TCP MD5/TCP-AO quando suportados, sem coletar
valores, hashes de credenciais ou linhas completas de configuração de chaves.
Usar mecanismo de coleta com redação quando configuração sensível for necessária.
Não desabilitar autenticação nem solicitar a chave do peer no chat.

Conferir famílias e capacidades efetivamente negociadas. Ausência de AF, GR,
add-path ou route-refresh tem efeitos distintos; não inferir suporte só pelo nome
do fabricante. O Hold Time é negociado; valores configurados e efetivos podem
diferir. Hold Time zero desabilita esse timer; não impor um default universal.

## Coleta mínima e limites

Começar por summary, detalhe do peer, rota até o peer e eventos no intervalo
da falha; ampliar conforme a hipótese. Preferir saídas estruturadas de parsers
existentes. Em CLI, preservar cabeçalhos e linhas de continuação IPv6, registrar
erro/truncamento e tratar ausência de amostra como desconhecido, não zero.

Uma captura, se autorizada, deve ter interface/direção/filtro, duração ou limite
de pacotes e tratamento privado definidos. Não coletar payload ou TCP options
desnecessários. Traceroute/ping devem ser curtos e autorizados; não fazer varredura
de porta ou habilitar debug/trace como substituto de uma evidência ausente.

Fontes: [máquina de estados e negociação, RFC 4271](https://www.rfc-editor.org/rfc/rfc4271.html),
[verificação de sessões Junos](https://www.juniper.net/documentation/us/en/software/junos/bgp/topics/topic-map/troubleshooting-bgp-sessions.html),
[segurança operacional BGP, RFC 7454](https://www.rfc-editor.org/rfc/rfc7454.html).

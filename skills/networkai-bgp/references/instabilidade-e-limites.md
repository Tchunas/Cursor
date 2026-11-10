# Quedas, limites e filas BGP

Construir uma linha do tempo do peer/família: uptime atual, última transição,
motivo/direção de reset e eventos próximos de interface, BFD, processo e sistema.
Contador acumulado de flaps sem intervalo/baseline não fornece taxa de quedas.
Um clear que recuperou o serviço não demonstra bug nem identifica a causa.

## Queda por limite de prefixos

Procurar evidência explícita como PrefixLimitIdle, Idle (PfxCt), Cease por limite
ou log equivalente da plataforma. Conferir configuração efetiva e qual contagem
é limitada: recebidos ou aceitos, família, limiar, warning-only, teardown e eventual
restart timer. Os mecanismos diferem por release; atingir o limiar de aviso não
significa necessariamente derrubar a sessão.

Separar aumento legítimo da tabela, mudança de policy e possível vazamento usando
origem, horário e amostra direcionada de prefixos. Um total elevado isoladamente
não comprova leak. Não aumentar limite, desligar proteção ou limpar sessão para
recuperar o serviço sem plano e autorização específicos.

## Timers, BFD e recursos

- Hold Timer Expired informa ausência de mensagens BGP válidas no intervalo
  negociado; distinguir perda, congestionamento, CPU/processo e peer remoto.
  Conferir contadores e horários antes de afirmar que o timer está errado.
- BFD Down só é relevante se BFD estiver habilitado e associado ao peer. Um campo
  disabled/down não prova falha nem exige ativação. Correlacionar o evento BFD
  com a queda BGP; não afrouxar timers automaticamente.
- Verificar erros/discards/óptica/MTU apenas nas interfaces do caminho identificado.
  CPU alta ou queda de interface contemporânea pode apoiar uma hipótese; sozinha
  não atribui causa ao BGP. Correlacionar CoPP/filas com tráfego e janela da falha.

## Filas e pressão de updates

Uma fila de saída positiva durante convergência não prova travamento. Comparar
poucas amostras espaçadas do mesmo peer, anotando fila, mensagens enviadas,
uptime e eventos. Crescimento persistente com pouco progresso merece investigar
transporte, processamento local e capacidade do receptor; não identifica sozinho
qual lado é responsável. Contadores resetados ou coletas de épocas diferentes
não podem ser usados como delta.

Se só houver um snapshot, entregar a conclusão limitada e o próximo recorte.
Evitar polling intenso, debug amplo ou reiniciar rpd/BGP. Para bug conhecido,
exigir modelo/release, sintoma compatível e referência oficial do fabricante;
recuperação após reset não é confirmação de defeito de software.

Fontes: [flags e estatísticas do peer Junos](https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/command/show-bgp-neighbor.html),
[subcódigos Cease, RFC 4486](https://www.rfc-editor.org/rfc/rfc4486.html),
[BFD, RFC 5880](https://www.rfc-editor.org/rfc/rfc5880.html).

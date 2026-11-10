# Sequência Nettools por função

Este fluxo deriva do exemplo fornecido pelo usuário. Ele não demonstra versões,
topologia física, disponibilidade, estado committed ou resultados de pós-check.
Os valores reais e o texto de origem ficam no perfil privado, fora do Git.

## 1. Identificar a entrega e resolver as lacunas

Registrar frontend e backend como serviços distintos: cliente/host, inventário,
interfaces, VLANs, VNIs, RTs, subnet/gateway, MTUs e domínios de forwarding.
Localizar o par de BLEAFs, LEAF da máquina, LEAF de transporte, CORE, par de AGGs
e par de TORs. A sequência do exemplo não comprova cabos, relação entre LAGs
ou necessidade de mudar todos os alvos em uma nova entrega.

Confirmar plataforma/versão e desenho efetivo, incluindo herança e inativos.
Preservar lacunas explícitas: portas de acesso dos TORs, membros físicos dos
LAGs, RT de backend, base EVPN, encaminhamento entre domínios, cadeia de exportação
e peers não estão demonstrados pelo recorte. Não inventar tradução entre VLANs.

## 2. Preparar o frontend em cada BLEAF

Conferir IRB com descrição, endereço/prefixo, MTU inet e MAC de gateway distribuído;
IRB na VRF L3; VLAN na MAC-VRF com VID, IRB, VNI e RT por VNI; membership da VLAN
no LAG de frontend previsto pelo perfil. Conferir ambos os BLEAFs individualmente.

O exemplo usa bloco maior que /31. Distinguir endereço de interface, rede do
prefixo e IPs do host, com conflito/ocupação e reserva verificados para a entrega.
Não aplicar a convenção de dois endpoints de um /31 a essa rede.

Preservar a semântica `upto /32` da entrada de exportação quando aprovada: permite
o prefixo base e seus mais específicos até /32, diferentemente de `exact`.
Essa condição não cria rotas nem comprova anúncios. Revisar rotas elegíveis,
cadeia efetiva da policy, famílias, comunidades e peers; não trocar por `exact`
nem ampliar cobertura sem decisão explícita do responsável pelo desenho.

## 3. Preparar o backend no LEAF da máquina

No modelo fornecido, criar ou completar VLAN/VNI L2 na MAC-VRF, binding explícito
da interface lógica, descrição e MTU física da porta, modo trunk, membership da
VLAN de backend e RSTP edge na porta do servidor.

Trunk no servidor não comprova native VLAN nem host sem tag: confirmar tagging,
subinterface/bridge e MTU no host. Não copiar o modo access do padrão frontend.
RSTP edge exige confirmar que é porta de endpoint, sem switch/bridge indevido;
não estender edge aos uplinks. MTU física e MTU inet são campos diferentes.

## 4. Preparar os trechos de transporte necessários

| Função do exemplo | Configuração descrita | Verificação necessária |
| --- | --- | --- |
| LEAF de transporte | VLAN/VNI de backend na MAC-VRF e VLAN no LAG | Confirmar domínio, membros e base EVPN/RT efetiva; o trecho não cria outro gateway |
| CORE Junos | VLAN de frontend no namespace global `vlans` e membership nos LAGs indicados | Não mover o objeto para MAC-VRF por analogia; conferir quais LAGs realmente compõem o caminho |
| Par de AGGs Cisco-like | Definição/nome da VLAN de backend e adição no port-channel | Preservar allowed VLANs existentes e conferir par, LACP e ambos os lados |
| Par de TORs Cisco-like | Somente definição/nome da VLAN de backend | Não declarar caminho de acesso pronto: o trecho não mostra portas/trunks do host ou uplinks |

No Cisco-like do exemplo, `switchport trunk allowed vlan add` é aditivo. Não
remover `add` e substituir a lista inteira. Conferir dialeto e suporte no modelo
real; nome de interface e comando de salvamento não são universais.

## 5. Produzir pacote e inversos contra a baseline

Por equipamento: aplicação, rollback específico, checks e hashes. Excluir sessões
de login e commits automáticos dos arquivos de delta. Incluir somente diferenças
comprovadas; repetir uma definição de VLAN não autoriza removê-la no rollback.

Para VLAN preexistente, restaurar nome/descrição e membership alterados; para
membership novo em trunk/LAG compartilhado, remover somente o membro acrescentado.
Uma definição de VLAN ausente antes só pode ser removida se ainda exclusiva da
entrega e sem dependência concorrente. Restaurar MTU, modo, descrição, edge e
marcadores inativos anteriores. Não usar exclusão de port-channel ou VLAN inteira
como inverso genérico de uma adição de membro.

Preparar ordem por dependências e stop/rollback por serviço. A ordem em que o
usuário listou os equipamentos não é uma sequência operacional validada.
Escrita em IPAM, candidato, commit, copy/save e teste ativo dependem de escopo
próprio. Reutilizar sessões autorizadas sem ampliar alvos ou operações.

## 6. Validar separadamente frontend e backend

- Frontend: estado e endereço da IRB nos BLEAFs, MAC/ARP, VLAN/VNI/RT, LAGs e
  resolução no host; rota ativa, next hop, anúncios efetivos e retorno por peer.
  Conferir os específicos que a policy pode aceitar, não somente o prefixo base.
- Backend: trunk/tagging do host, MTU do caminho, VLAN/VNI, binding à MAC-VRF,
  EVPN/VTEPs e aprendizagem MAC; não exigir rota IPv4 em segmento puramente L2.
- Transporte: VLAN permitida/existente e estado em cada ponta/LAG/par; RSTP e
  erros/discards. VLAN criada no TOR não prova membership ou acesso do servidor.
- Aceite: teste host/aplicação e retorno com origens e serviço identificados,
  quando autorizado. Sem essas provas, relatar o que está configurado/observado
  e o que ficou pendente; não declarar ativação concluída a partir dos scripts.

# Roteiro por fluxo

## Encaminhamento e publicação

1. Fixar origem, destino, protocolo, portas, interface e contexto de entrada.
   Separar pacote destinado ao próprio FortiGate (local-in/admin) de trânsito.
2. Resolver objetos/grupos e VIP/port-forward. Uma VIP existente não comprova
   que uma política ativa permita entrada. Com Central NAT, conferir a semântica
   da release para matching do endereço interno após DNAT; não generalizar para
   outros modos ou versões a partir de um único caso.
3. Reconstruir decisão de saída com policy routes, rotas específicas, conectadas,
   rotas de túnel, blackhole e default. Split-tunnel no cliente não prova rota,
   seletor ou permissão no firewall. Uma rota mais específica pode mudar o caminho.
4. Conferir política efetiva na ordem configurada, identidade/grupo, schedule,
   serviço e estado. ID não é prioridade; retorno de sessão stateful não equivale
   a iniciar uma conexão reversa. Ausência de log de deny não exclui descarte.
   Conferir a legenda da fonte: “ativa” pode representar uso em uma janela
   histórica, e “nunca usada” não significa policy desabilitada. Não presumir
   exigência de grupo em todas as saídas porque a maioria das policies o exige.
5. Determinar modo NAT. Em Central SNAT, percorrer a tabela na ordem real e
   conferir interfaces, origem/destino, protocolo e pool; `nat disable` na policy
   não implica saída sem tradução. Diferenciar pool de saída, IP da interface e
   caso sem regra correspondente. No NAT por política, avaliar essa política.
6. Analisar tuple de retorno, rota reversa, estado da sessão e firewall do host
   ou hipervisor. A origem após SNAT pode ser justamente a condição de um bloqueio.

Para IPsec, cruzar seletores dos dois lados e NAT de um eventual segundo gateway.
Um bloco público pode sair pelo túnel em vez da WAN. Exceção de policy routing
para um servidor não vale para todos, nem para blocos fora da lista da exceção.
Expansão de rede pode depender de objetos/grupos, distribuição de rotas ao cliente,
seletores espelhados e NAT remoto; isso é plano, não autorização para aplicar.

## VPN e identidade

Separar transporte IKE, autenticação de gateway, EAP/SAML do usuário, associação
de grupo, atribuição de IP/DNS, rotas instaladas e autorização de tráfego. Grupo
no FortiGate que contém um servidor SAML não prova filtro por grupo no IdP.
Avaliar metadados e bindings sem reproduzir PSKs, tokens ou assertions.

Mapear dependências de endpoint público, proxy, IdP, DNS e seus hosts físicos.
Um serviço ligado diretamente à WAN pode compartilhar o host de um nó HA sem
ser protegido pelo failover do firewall. Não inferir esse comportamento só pelo
nome de VM, IP ou presença de um cluster.

## HA e revisão documental

Registrar membros, papéis na data da fonte, prioridade/override, heartbeat,
sincronismo e interfaces monitoradas. Link virtual up não comprova cabo físico
up. Distinguir disponibilidade da VM, bond, switch, operadora e dependências.
Preservar diferenças de bond/LACP entre hosts; não normalizá-las em documentação.

Revisar objetos publicados amplamente, policies sem perfil de segurança, acesso
administrativo remoto/trusthosts, validade de suporte e regras sem uso como
itens documentais para revalidação, nunca como autorização para hardening.
Não apagar regra sem hits nem afirmar vulnerabilidade/exploração sem evidência.

## Procedência

Método sintetizado a partir de documento operacional privado fornecido pelo
usuário e das fronteiras existentes do NetworkAI. A fonte integral e a relação
entre afirmações e páginas ficam no registro privado do projeto. Não inclui
conteúdo de manual de fabricante nem validação da sintaxe em uma release atual.
Não há dependências executáveis novas.

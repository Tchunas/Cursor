---
name: networkai-contingency-access
description: "Avaliar VPN de contingência ou Last Resort: alcance real, DNS e identidade compartilhados, origem após NAT, firewall de hipervisor e dependências que impedem recuperação pelo acesso alternativo."
---

# Acesso de contingência

Use para decidir o que um acesso alternativo permite recuperar. Uma operadora
independente não torna independentes DNS, IdP, energia, switches ou serviços.
Use os artefatos autorizados; fonte histórica não é teste de contingência.

Localize o registro privado da fonte no projeto (`knowledge/company/`, título e
manifesto de proveniência). Preserve ali endereços, máquinas, usuários, datas e
resultados. Sem ele, não invente o perfil do ambiente. A skill guarda o método.

1. Comparar principal e contingência: endpoint, transporte, autenticação, pool,
   DNS entregue, split-tunnel, rotas, políticas e SNAT por interface de saída.
   Pool igual não prova mesmo acesso. Diferenciar rede conectada, rede anunciada
   ao cliente, rede roteada e serviço permitido.
2. Separar acesso por IP de resolução de nomes. Se o DNS entregue está no
   componente que falhou, registrar dependência residual mesmo com túnel ativo.
   Rastrear também dependências de login/SSO e hospedagem do proxy/IdP.
3. Identificar a origem vista pelo destino em cada segmento. Não assumir um
   único IP de SNAT para todas as saídas. Conferir firewall da VM, flag da NIC,
   grupo de isolamento e ACCEPT anterior. Grupo presente não significa aplicado;
   uma exceção TCP não libera ICMP nem outros serviços.
4. Separar gerência do próprio firewall e tráfego encaminhado. SNAT de trânsito
   não determina automaticamente o endereço que um trusthost verá no local-in.
5. Produzir matriz destino/serviço, caminho, origem traduzida, dependências,
   resultado documental, teste necessário e confiança. Redes de outros sites,
   consoles fora dos prefixos conectados e serviços sem exceção não ficam
   recuperáveis apenas por haver um gateway alternativo.

Relatar inconsistências entre diagramas, tabelas e narrativa, mantendo a página
de cada afirmação. Contagem de VMs não substitui lista completa nem comprova
quantas bloqueiam todo o tráfego ou só parte dos serviços.

Plano de teste deve cobrir login, acesso por IP e por DNS, serviço permitido e
controle que deve continuar bloqueado. Definir limites e coleta autorizada; não
desligar primário, alterar DNS/rotas, abrir isolamento ou forçar failover para
demonstrar a contingência. Para mudanças, preparar impacto, rollback e validação
antes da execução explicitamente autorizada.

Método sintetizado de fonte operacional privada fornecida pelo usuário. Fonte
integral e páginas ficam no projeto, fora da biblioteca publicável. Não inclui
credenciais, dependências executáveis novas ou comprovação de operação atual.

## Exemplo e aceite offline

Entrada fictícia: a VPN alternativa conecta; entrega como DNS o firewall
principal indisponível; só anuncia duas redes locais. Um serviço permite a
origem traduzida do principal, mas bloqueia a origem do gateway alternativo.

Aceite: separar falha de DNS do alcance por IP, identificar a origem após NAT
por saída e a regra do destino, e não prometer acesso a um terceiro site.
Não abrir isolamento, trocar DNS nem desligar o principal para testar. Produzir
matriz com serviço, evidência documental e teste pendente. Exemplo revisado
documentalmente; sem avaliação independente de modelo ou execução na rede.

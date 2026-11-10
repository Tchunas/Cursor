---
name: networkai-routing
description: Investigar roteamento IP entre protocolos, rotas estaticas, OSPF e evidencias locais do NetworkAI; localizar configuracao e fatos para diagnosticos de caminhos e planos de mudanca.
---

Leia as instruções e os contratos aplicáveis à tarefa. Se houver um checkout NetworkAI autorizado, reutilize o cliente Oxidized, parsers, ConfigFactStore e resolução canônica existentes. Fora desse ambiente, use as evidências fornecidas e declare as lacunas; esta skill não depende de conexão nem de diretórios privados. Para sessoes e anuncios BGP, usar [networkai-bgp](../networkai-bgp/SKILL.md); para avaliar cadeias de import/export, usar [networkai-routing-policies](../networkai-routing-policies/SKILL.md), carregando somente o procedimento necessario.

## Rastrear uma rota ou peer

Se disponíveis, válidos e autorizados, consulte o índice de análise de configurações e a geração correspondente para localizar sinais de BGP, policy, VRF, OSPF, IS-IS ou MPLS. Indice ausente ou invalido nao comprova ausencia de configuracao: use a identidade resolvida e o cliente/mirror existente no escopo autorizado. Os sinais sao candidatos textuais; selecione o dispositivo exato e consulte seu backup com `MirrorClient`. Preserve data do backup e hash da evidencia sem expor inventarios ou dumps.

No armazenamento de fatos autorizado, consulte os fatos do dispositivo para localizar vizinhos, ASNs, policies, VRFs e rotas estaticas. Revise a configuracao relevante: parsers podem extrair apenas nomes de policies, perder heranca de grupos ou omitir a instancia de um peer. Nunca atribua uma instancia ou policy efetiva apenas pela ausencia de um campo.

Identifique instancia, familia de enderecos, vizinho e ASNs. Rastreie import/export do peer e do grupo, heranca, ordem de termos, objetos referenciados, prefix filters, communities, AS path, local preference, next hop e rotas static/discard. Confira `inactive:` e `deactivate`, alem de grupos Junos e filtros Cisco aplicaveis. Documente partes nao resolvidas.

Em Junos, diferencie configuração candidata, committed, inativos, herança e operação; confira cada visão necessária na plataforma e release do alvo. Descobrir peers/familias na configuracao efetiva, inclusive grupos herdados; ausencia na raiz nao comprova ausencia do peer. Preservar a ordem real dos termos: nomes numericos nao ordenam a avaliacao. Um aceite IPv6 depois de rejeicao terminal pode ser inalcançavel; confirmar anuncios antes de concluir impacto e nao copiar esse erro de uma referencia.

Configuracao de exportacao nao comprova anuncio efetivo; uma rede configurada nao comprova rota ativa. Se a tarefa autorizar verificacao operacional, prepare consultas de leitura apropriadas a plataforma e versao para sessao BGP, RIB da instancia, rotas recebidas/anunciadas, resolucao de next hop e FIB. Sem essa evidencia, mantenha a conclusao limitada ao backup; nao infira direcao de trafego.

## Planejar uma alteracao

Quando solicitado, reutilize a policy existente e prepare a menor mudanca que satisfaz o objetivo. Vincule comandos ao dispositivo, instancia, peer e prefixo exatos. Inclua pre-checks, dependencias, impacto sustentado por evidencia, rollback adequado a plataforma e validacao de peers e rotas depois da mudanca. Verifique suporte antes de propor commit confirmed ou checkpoints.

A skill orienta investigacao e planejamento dentro da autorizacao vigente. Execucao exige autorizacao especifica. Reutilizar autenticacao valida; quando houver necessidade de entrada humana no Windows, usar terminal Ubuntu WSL visivel; em macOS/Linux, usar terminal local visivel equivalente, com entrada oculta e segredos apenas em memoria conforme as regras globais. Fatos mutaveis devem ser revalidados na tarefa futura.

## Exemplo offline e critério de aceite

Entrada sintética: Uma rota sintética 192.0.2.0/24 aparece em configuração de exportação; não há RIB ou anúncio observado.

Aceite: Distinguir intenção de anúncio efetivo, localizar os fatos disponíveis e pedir apenas a evidência faltante do alvo autorizado. Não afirmar anúncio, tráfego ou impacto.

O exemplo é um caso para revisão, não evidência operacional ou resultado de avaliação de modelo. Testar mudanças somente em contexto autorizado.

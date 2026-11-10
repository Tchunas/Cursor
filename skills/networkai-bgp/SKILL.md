---
name: networkai-bgp
description: "Diagnosticar sessões BGP e prefixos recebidos ou anunciados, elegibilidade de exportação, seleção de caminhos e next hop. Usar para falhas de peering ou de anúncio de uma rota específica."
---

# Diagnóstico BGP

Identificar dispositivo, plataforma/versão, instância/VRF, AFI/SAFI, peer/ASNs,
prefixo e sentido pretendido. Em NetworkAI, reutilizar conectores, fatos e
identidades conforme [networkai-routing](../networkai-routing/SKILL.md).
Fora desse ambiente, trabalhar com as evidências fornecidas e declarar lacunas.
Backup datado, saída atual e simulação são classes de evidência diferentes.

## Escolher o recorte

Para sessão que não estabelece, consultar
[sessões e transporte](references/sessoes-e-transporte.md): estado, origem,
alcance TCP, negociação e último erro. Para quedas, limites de prefixos ou filas,
consultar [instabilidade e limites](references/instabilidade-e-limites.md).
Escolher consultas que distingam as hipóteses; não coletar tabelas completas
quando um peer, família, intervalo de tempo ou prefixo resolve a pergunta.

## Seguir a rota, não apenas a sessão

- Conferir estado e família negociada do peer. Established não demonstra
  aceitação, melhor caminho ou anúncio do prefixo pedido.
- Separar recebido, aceito, selecionado na RIB, exportado ao peer e instalado
  na FIB. Preferência entre protocolos e local preference BGP são etapas
  distintas; uma community da rota inativa não pertence à rota ativa.
- Conferir policy efetiva do neighbor/grupo, herança, inativos e objetos
  referenciados. Para avaliação detalhada, usar
  [networkai-routing-policies](../networkai-routing-policies/SKILL.md).
- Verificar resolução do next hop na tabela correta, políticas de origem,
  AS loop, communities de controle e regras iBGP/route reflection aplicáveis.
  Não assumir que toda rota aprendida via iBGP será retransmitida a outro iBGP.
- Correlacionar o anúncio pós-policy com o prefixo exato e a família corretos.
  Saída vazia só sustenta ausência nessa consulta se alvo, sintaxe, tabela,
  recorte e sucesso estiverem confirmados. Distinguir erro ou coleta truncada.
- Para alcance de um host, verificar longest-prefix match e retorno. Um /24
  discard usado para originar anúncio pode coexistir com um /27 que encaminha.
  Anunciar por um trânsito não força o tráfego de saída a usar esse trânsito.

Ler [Junos](references/junos.md), [NX-OS](references/nxos.md) ou
[IOS/IOS-XE](references/iosxe.md) conforme o equipamento; consultar
[references/interpretacao.md](references/interpretacao.md) para decidir a
próxima evidência quando sessão, policy e encaminhamento divergem.

## Entrega e mudanças

Relatar o ponto comprovado da cadeia e a próxima consulta necessária, com
origem e horário. A elegibilidade local para anúncio não comprova aceitação
pelo provedor nem propagação global. Não concluir filtragem remota só por timeout.

Quando solicitado, planejar o menor delta nos objetos existentes, com baseline,
efeito nos demais prefixos/famílias, rollback específico e pós-checks. Consultas
externas dependem do escopo vigente; clear/refresh de BGP, commit e mudanças
exigem autorização de execução. Reutilizar a autenticação válida conforme as
regras globais; nunca pedir segredos no chat.

Soft clear e route refresh também são operações ativas: não executá-los como
coleta de leitura. Não habilitar retenção de rotas, trace/debug, mudar timers,
limites, origem, autenticação ou ACL para completar o diagnóstico. Verificar
autenticação por estado/metadados sem reproduzir senhas ou chaves. Usar apenas
capturas e sondas limitadas cobertas pelo escopo vigente.

Consultar [fontes e procedência](references/fontes-e-procedencia.md) para as
bases técnicas e os limites do material externo adaptado.

## Exemplo offline e critério de aceite

Entrada sintética: Um peer fictício está Established; o prefixo sintético 192.0.2.0/24 só aparece numa policy de exportação.

Aceite: Não equiparar sessão a anúncio. Seguir elegibilidade, RIB, next hop e evidência pós-policy do peer/família exatos; não executar clear ou refresh.

O exemplo é um caso para revisão, não evidência operacional ou resultado de avaliação de modelo. Testar mudanças somente em contexto autorizado.

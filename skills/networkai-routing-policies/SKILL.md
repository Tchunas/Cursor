---
name: networkai-routing-policies
description: "Avaliar policies de roteamento, import/export, prefix-lists e route-maps: ordem, matches, communities, herança e efeito de deltas. Usar para explicar ou planejar o tratamento de rotas por uma policy."
---

# Avaliação de policies de roteamento

Fixar dispositivo/plataforma, versão, instância, família, ponto de aplicação,
direção e a rota com seus atributos naquele ponto. Para localizar evidências
NetworkAI, reaproveitar [networkai-routing](../networkai-routing/SKILL.md).
Para dados de pacotes, avaliar filtros no ponto, direção e fluxo pertinentes:
uma policy de BGP não é uma ACL de tráfego. Se faltar evidência do filtro efetivo,
registrar a lacuna e manter a conclusão limitada à política de rotas.

## Fazer uma avaliação rastreável

1. Resolver o vínculo efetivo no neighbor/grupo/protocolo, incluindo templates,
   grupos, inativos e a cadeia completa. Preservar a ordem real; nomes como
   TERM_100 e TERM_20 não determinam a ordem de avaliação.
2. Resolver prefix lists, filtros, communities, AS paths e chamadas de policy.
   Objeto ausente do parser é referência não resolvida até conferir a fonte.
3. Seguir cada termo: atributos de entrada, resultado do match, transformações
   e controle de fluxo. Respeitar a semântica do fabricante para condições,
   comprimentos de prefixo e permit/deny dentro de objetos auxiliares.
4. Aplicar término, continuidade e default da policy/cadeia/protocolo corretos.
   Uma policy posterior de deny-all não desfaz um accept terminal anterior.
5. Produzir trilha curta dos termos alcançados e a conclusão configurada.
   Para anúncio real, conferir também elegibilidade e Adj-RIB-Out conforme
   [networkai-bgp](../networkai-bgp/SKILL.md).

Ler [references/junos.md](references/junos.md) para termos/cadeias Junos ou
[references/nxos.md](references/nxos.md) para route-maps/prefix-lists NX-OS.
Para validar candidato offline, usar laboratório ou simulador autorizado,
mantendo atributos de entrada e controles fora do escopo comparáveis à baseline.
Registrar limitações do simulador; não executar testes remotos ou mudanças.

## Planejar sem ampliar o efeito

Quando solicitado, reutilizar objetos existentes e vincular o delta à baseline
exata. Avaliar pelo menos a rota alvo, uma rota fora do escopo e cada família
afetada. Transformar um reject genérico em reject de dois prefixos muda também
o destino das demais rotas; um accept em termo genérico pode ampliar exportação.

Se a restrição de não criar objetos impedir uma solução limitada, explicar a
incompatibilidade e preparar alternativas para revisão. Não contornar o limite
alterando um default global ou o valor de uma community compartilhada.

Preparar inverso das alterações da tarefa, mantendo o que já existia. Não usar
rollback de uma revisão inteira sem conhecer mudanças concorrentes. Sintaxe
válida, simulação e commit check não comprovam anúncio nem alcance. Execução,
inclusive testes remotos que alterem estado, depende de autorização específica.

## Exemplo offline e critério de aceite

Entrada sintética: Uma cadeia sintética contém accept terminal para 192.0.2.0/24 antes de deny-all posterior.

Aceite: Preservar a ordem e reconhecer o término no accept; não concluir que o deny posterior desfaz o aceite nem que esse resultado comprova anúncio real.

O exemplo é um caso para revisão, não evidência operacional ou resultado de avaliação de modelo. Testar mudanças somente em contexto autorizado.

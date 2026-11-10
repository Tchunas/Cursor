---
name: networkai-igp
description: Diagnosticar OSPFv2/OSPFv3, IS-IS e EIGRP; analisar adjacências, LSDB/topologia, redistribuição e instalação de rotas em Cisco e Juniper.
---

# IGP: vizinhança, cálculo e encaminhamento

Use para um protocolo interior identificado, inclusive exercício ou revisão offline.
Comece pelo equipamento/OS/release, instância/VRF, família, interfaces, vizinhos e
prefixo afetado. Evidência fornecida pode bastar; não exigir conexão para explicar
um cenário. Uma configuração, LSDB ou vizinhança isolada não prova encaminhamento.

Escolha a referência conforme a pergunta:

- [OSPF e OSPFv3](references/ospf.md): estados, parâmetros, LSAs e rotas externas.
- [IS-IS e EIGRP](references/isis-eigrp.md): níveis, topologias e DUAL.
- [Coleta Cisco/Juniper](references/plataformas.md): comandos de leitura e contexto.
- [Fontes e procedência](references/fontes.md): fundamentos e limites do material.

Relacione interface/transporte do protocolo -> vizinhança -> base/topologia ->
cálculo -> RIB -> resolução do next hop/FIB -> retorno. Localize a primeira
divergência sustentada por evidência, sem atribuir causa pela aparência de um estado.
Compare intenção com parâmetros efetivos dos dois lados quando disponíveis.

Para rotas ausentes, conferir alcance da LSA/LSP, área/nível, seleção de protocolo,
preferência administrativa, sumarização, filtros e redistribuição. Uma entrada
perdendo na RIB não exige alterar métricas do IGP; o motivo pode ser outra origem.
Não redistribuir tudo para contornar falta de uma rota; identificar ponto, direção,
seed metric/tipo, tags, filtros e risco de realimentação. Reutilizar
[policies](../networkai-routing-policies/SKILL.md) quando a falha estiver na política.

Separar fatos, hipóteses, lacunas e próxima verificação. Ao criar um exercício de
laboratório, fornecer topologia sintética, resultado esperado e, se o exercício
pedir falha, sua injeção e critério de recuperação. Não inventar falha em um
cenário saudável fornecido.
Não alegar cobertura integral de certificação ou compatibilidade de toda release.

Esta skill não autoriza acesso nem alterações. Consultas live usam somente os
alvos e operações autorizados, com saída limitada e credenciais protegidas.
Clear de processo/vizinho, refresh, BFD, mudança de timer, MTU, autenticação,
redistribuição ou trace/debug exigem escopo específico. Não executar reset como
primeiro diagnóstico nem alterar o lado que parece fora do padrão sem conhecer
a intenção. Em planos solicitados, incluir menor delta, dependências, rollback
adequado à plataforma e pós-check independente.

## Exemplo offline e critério de aceite

Entrada sintética: Dois DROthers fictícios mostram OSPF 2-Way numa rede broadcast; não há outra evidência de falha.

Aceite: Explicar que 2-Way pode ser normal; conferir papéis DR/BDR e desenho antes de atribuir defeito. Não resetar processo ou mudar timers.

O exemplo é um caso para revisão, não evidência operacional ou resultado de avaliação de modelo. Testar mudanças somente em contexto autorizado.

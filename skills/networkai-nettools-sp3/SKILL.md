---
name: networkai-nettools-sp3
description: Preparar ou revisar ativação Nettools em SP3, separando frontend com gateway nos BLEAFs, backend L2 nos LEAFs e transporte Junos/Cisco-like, pelo perfil privado fornecido.
---

# Nettools em SP3

Usar para Nettools conforme o modelo fornecido pelo usuário. Ler
[sequência e aceite](references/fluxo.md). Localizar os parâmetros e o exemplo
original em `knowledge/company/nettools-sp3-model.md` no checkout de automação,
ou no handoff privado restaurado. Registrar fonte e revisão; o exemplo é intenção
de configuração fornecida, sem prova de aplicação ou operação atual.

Separar frontend, backend da máquina e transporte. Não aplicar automaticamente
ao backend o molde frontend /31 da skill `networkai-sp3-activation`. O trecho
Nettools põe gateway nos BLEAFs e backend L2 nos LEAFs; ele não documenta gateway
de backend nem IRB de frontend no LEAF da máquina. Não acrescentar esses objetos
por analogia, nem concluir que inexiste configuração não mostrada.

Confirmar identidades, versões, interfaces/LAGs, domínio de cada VLAN, VNI, RT,
gateway, prefixo, exportação e dependências por evidência autorizada antes do delta.
O nome de um site ou o mesmo nome de LAG não prova ligação física. Se faltar
perfil/base/interface essencial, manter essa parte pendente e produzir o restante
do plano. Não consultar fontes ou executar comandos apenas por possuir a skill.

Produzir delta, inverso e pré/pós-checks por alvo, com Junos e Cisco-like em
arquivos distintos. Reutilizar estruturas existentes e a baseline; trunk deve
preservar todas as VLANs anteriores. Os comandos `configure`, `commit check`,
`commit and-quit`, `conf t` e salvamento do exemplo não autorizam execução.

Para planejamento de uma entrega nova, procurar primeiro VLAN nova e não usada;
os IDs do caso são referência histórica, não reserva nem padrão de alocação.
Para manutenção de serviço existente, preservar IDs aprovados e não renumerar
automaticamente. Não ampliar a regra para outros sites ou outros tipos de serviço.

Usar as skills existentes de scripts/validação de ativação, Junos, Cisco,
EVPN/VXLAN, Layer 2 e routing policies somente nas etapas necessárias e quando
disponíveis. Sem suporte da plataforma confirmado, entregar plano com a lacuna.

Separar configuração proposta, aplicada, observada e teste do host. A aprendizagem
é documental; não representa treinamento do modelo, deploy ou validação de rede.

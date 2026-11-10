# Avaliação Junos

Preservar committed com inativos e herança efetiva como visões distintas.
Distinguir candidato de configuração ativa e registrar origem e horário.
Confirmar ordem por configuração hierárquica/efetiva, incluindo `insert` e grupos;
não ordenar termos alfabeticamente ao converter SETs em um relatório.

`accept` e `reject` encerram a avaliação de uma cadeia comum. `next term` e
`next policy` têm efeitos diferentes. Expressões booleanas e subrotinas exigem
suas próprias regras: não tratá-las como uma lista linear de policies.
Termo sem match pode atingir todas as rotas elegíveis. A ausência de ação terminal
não significa rejeição automática: seguir o fluxo e o default do protocolo.

Condições de tipos diferentes, como community e prefixo, precisam ser avaliadas
em conjunto. Para várias entradas do mesmo tipo, route-filter, `exact`,
`orlonger`, comprimentos e ações inline, conferir a semântica específica. Não
substituir esse comportamento por um OR/AND universal. `community set`, `add` e
`delete` têm efeitos distintos sobre atributos usados em termos posteriores.

No delta, verificar se retirar um match faz o termo atingir mais rotas, se o
reject final continua abrangente e se o aceite está antes de um término geral.
Para retorno, restaurar os valores e a ordem anteriores apenas dos objetos
alterados; não apagar um objeto que já existia inativo.

Fontes: [avaliação de policy](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-routing-policies-evaluation-method.html),
[defaults](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-routing-policies-actions-defaults.html),
[expressões](https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-applying-policy-expressions-to-routes-exported-from-routing-tables.html).

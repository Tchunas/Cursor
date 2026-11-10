# Contribuir com skills-do-time

1. Leia AGENTS.md e a [política central](https://github.com/EveoNetwork/padroes-e-documentacao/blob/main/POLITICA-IA.md). Confira o [catálogo](https://github.com/EveoNetwork/padroes-e-documentacao/blob/main/CATALOGO.md).
2. Pesquise neste repo e nos repos relevantes antes de criar. Registre termos, resultados e a decisão de ampliar ou criar em uma Issue. Defina um responsável e atribua a Issue; coordene tarefas paralelas.
3. Atualize main local e crie uma branch por tarefa: git switch -c feat/NUMERO-resumo. Nunca use main como área de trabalho.
4. Implemente, execute testes proporcionais e atualize catalogo.json. Liste componentes com id, path, purpose, owner, acceptance e aliases opcionais. IDs e aliases devem ser únicos, inclusive diferenças de caixa, hífen e underscore.
5. Execute python3 scripts/check_governance.py e python3 -m unittest discover -s tests -p 'test_*.py' -v.
6. Abra PR para main usando o template e peça revisão humana. CI valida requisitos declarados, não a veracidade da busca/leitura nem duplicação semântica.

A branch main representa conteúdo aprovado; não publica nem instala automaticamente. O plano Free não impõe proteção de branch em privados: o fluxo de PR é uma regra operacional até habilitar proteção em um plano compatível. Não registre revisão apenas para cumprir formulário.

Hooks locais opcionais: git config core.hooksPath .githooks. Eles bloqueiam push direto a main neste clone, podem ser contornados e não são proteção de servidor. Remova com git config --unset core.hooksPath. Nunca sobrescreva configuração de hooks existente sem conferir.

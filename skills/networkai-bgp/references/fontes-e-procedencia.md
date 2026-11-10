# Fontes e procedência da ampliação BGP

Revisão de conteúdo em 23/10/2026. Os procedimentos locais foram redigidos para
NetworkAI, conferindo afirmações em documentação de fabricante e RFCs. Links
externos são material de consulta, não instruções para instalar ou executar.
Não foram importados scripts, instaladores ou o parser por regex dos exemplos.

## Skills externas consultadas

| Fonte | Identidade do conteúdo consultado | Uso e ressalvas |
| --- | --- | --- |
| [ECC: network-bgp-diagnostics](https://github.com/affaan-m/ECC/blob/main/skills/network-bgp-diagnostics/SKILL.md) | Git blob `f3e0fdbbfe219b7a00a92d400af0bf45bf8a3100` | Inspiração para triagem de sessão/transporte e leitura antes de resets; validação própria por plataforma. |
| [arsallls: network-bgp-diagnostics](https://github.com/arsallls/claude-network-skills/blob/main/skills/network-bgp-diagnostics/SKILL.md) | Git blob `4e0a2178e9f31160bf16d524e345f2a7d070b2f1` | Revisado, sem importação. Não adotar clear/retention como leitura, interpretação de `^$` como iBGP ou `^ASN$` como qualquer caminho com essa origem. |
| [domain-expert: juniper-junos](https://github.com/chrishuffman5/domain-expert/blob/main/plugins/networking/skills/juniper-junos/SKILL.md) | Caminho upstream do item Skills Directory; referências lidas na data da revisão | Usar organização modular como referência; verificar defaults BGP e sintaxe no fabricante. Não importar automações de commit/debug. |

Os hashes Git acima identificam bytes de arquivos, não aprovação de toda a
coleção nem garantia sobre futuras alterações em `main`. Uma nova versão externa
precisa de revisão. Como não há cópia de implementação de terceiros neste lote,
as licenças de suas coleções não são atribuídas aos nossos procedimentos.

## Bases técnicas

- [RFC 4271](https://www.rfc-editor.org/rfc/rfc4271.html): estados, OPEN,
  NOTIFICATION e timers; não define defaults universais de cada CLI.
- [RFC 7454](https://www.rfc-editor.org/rfc/rfc7454.html): proteções operacionais
  de BGP e transporte; aplicar conforme arquitetura e autorização.
- [RFC 4486](https://www.rfc-editor.org/rfc/rfc4486.html): motivos Cease, incluindo
  excesso de prefixos; conferir mapeamento de logs na plataforma.
- [RFC 5880](https://www.rfc-editor.org/rfc/rfc5880.html): BFD; estar disponível não
  significa estar habilitado no peer.

As referências por plataforma contêm documentação de comandos e defaults.
Os cenários sintéticos e a revisão externa acompanham a documentação do projeto
no pacote de fontes; a skill instalada separadamente não depende desse pacote.

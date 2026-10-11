# Documentação da Velqi

Índice de toda a documentação do projeto. Cada pasta tem uma responsabilidade única. Assim, quem procura "o quê" vai para `produto`, quem procura "como" vai para `arquitetura` e `implementacao`.

## Produto: o quê e por quê

| Documento | Para que serve |
| --- | --- |
| [Visão geral e significado do nome](produto/visao-geral.md) | O que é a Velqi, o nome, a personalidade e as premissas |
| [Sistema de referência (QA Agent NQD)](produto/sistema-de-referencia-nqd.md) | Leitura do sistema que inspirou o projeto |
| [Escopo e personas](produto/escopo-e-personas.md) | MVP x fases seguintes e para quem é |
| [Requisitos funcionais](produto/requisitos-funcionais.md) | 47 requisitos em 8 grupos |
| [Requisitos não funcionais](produto/requisitos-nao-funcionais.md) | Segurança, auditoria, desempenho, acessibilidade |
| [Critérios de aceite do MVP](produto/criterios-de-aceite-mvp.md) | Quando o MVP está pronto e perguntas em aberto |

## Arquitetura: como funciona

| Documento | Para que serve |
| --- | --- |
| [Visão arquitetural](arquitetura/visao-arquitetural.md) | Camadas, stack e estrutura do console |
| [ADRs](arquitetura/adr/README.md) | Decisões registradas, com contexto e consequências |

## Design: como parece

| Documento | Para que serve |
| --- | --- |
| [Identidade visual](design/identidade-visual.md) | Logo, cores, tokens, tipografia e regras de contraste |
| [Protótipo](design/prototipo.md) | Telas do console e como navegar |

## Prompts: como pedir ao agente de código

| Documento | Para que serve |
| --- | --- |
| [Visão geral dos prompts](prompts/README.md) | Como usar e manter os prompts |
| [Criação](prompts/criacao.md) | P1–P3: persona, novas skills, roteamento |
| [Implementação](prompts/implementacao.md) | P4–P7: console, HITL, API, MCP |
| [Validação](prompts/validacao.md) | P8–P10: revisão de skill, cenários, console |

## Implementação: como construir

| Documento | Para que serve |
| --- | --- |
| [Roadmap](implementacao/roadmap.md) | Fases, entregas e ordem de construção |
| [Organização do repositório](implementacao/organizacao-do-repositorio.md) | Onde cada coisa vai morar quando o código chegar |
| [Glossário](implementacao/glossario.md) | Termos do projeto (HITL, gate, skill, L1–L4…) |

## Como manter esta documentação

- Docs mudam no mesmo PR que muda o comportamento que elas descrevem.
- Decisão de arquitetura nova vira ADR. Uma decisão não se edita depois de aceita: ela é substituída por outra ADR.
- O CI valida o Markdown e os links em todo PR (ver `.github/workflows/docs.yml`).

# Roadmap

O caminho vai do mais barato de mudar (documentos e decisões) ao mais caro (código integrado). Cada fase só começa quando o critério de saída da anterior for cumprido.

> **Por que nessa ordem:** é como construir uma casa. Primeiro a planta (docs e ADRs), depois a fundação (catálogo de skills e guardrails, que valem mesmo sem console), depois as paredes (API) e só então o acabamento (console). Mudar a planta custa uma borracha; mudar a fundação custa uma obra.

## Fase 0: Descoberta e especificação (atual)

| Entrega | Status |
| --- | --- |
| Requisitos funcionais e não funcionais | Feito |
| Identidade visual e protótipo | Feito |
| Estrutura de docs, CI de docs e padrões do repositório | Feito |
| ADR 0004 (linguagem do BFF) e ADR 0005 (onde o agente roda) | Pendente |
| Squad piloto e projeto web do MVP definidos | Pendente |

**Critério de saída:** ADRs 0004 e 0005 aceitas e squad piloto definida.

## Fase 1: Catálogo de skills (sem console)

O catálogo já entrega valor sozinho, rodando no Claude Code ou no Copilot do dev.

| Entrega | Requisitos | Prompt |
| --- | --- | --- |
| `velqi-core` (persona, G1–G6, GP1–GP3) | RF-B01, RF-E01–E05 | P1 |
| Roteamento (`_index`, `_routing`, golden set) | RF-B02, RF-B04 | P3 |
| Skills do MVP: project-discovery, test-design, web-automation, iuclick, test-data-management | RF-C01–C05 | P2 |
| `velqi_verify_plan` (MCP) | RF-E05 | P7 |
| Validador de skills + CLI `velqi` | RF-A03, RF-A05, RF-A06 | P2 |

**Critério de saída:** um card real vira cenários e um PR com Playwright, rodando localmente, com o golden set em pelo menos 90%.

## Fase 2: API (BFF)

| Entrega | Requisitos | Prompt |
| --- | --- | --- |
| Máquina de estados + persistência | RF-C06 | P6 |
| Eventos SSE + auditoria | RF-E07, RNF-06 | P6 |
| Integração do runner com a API | ADR 0005 | P6 |

**Critério de saída:** uma execução completa registrada na API, com todos os eventos auditáveis.

## Fase 3: Console Angular

| Entrega | Requisitos | Prompt |
| --- | --- | --- |
| Scaffold, tokens, auth, temas | RF-H01, RNF-01, RNF-15 | P4 |
| Aprovação HITL | RF-H04, RF-D01–D03 | P5 |
| Detalhe da execução, catálogo, auditoria, sobre | RF-H03, RF-H05, RF-H06, RF-H08 | P4 |

**Critério de saída:** [critérios de aceite do MVP](../produto/criterios-de-aceite-mvp.md) cumpridos com a squad piloto.

## Depois do MVP

Integrações da Fase 2 do produto (Hopper, Sonar, Figma, API testing) e da Fase 3 (mobile, performance, chaos), conforme o [escopo](../produto/escopo-e-personas.md).

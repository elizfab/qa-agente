# Glossário

| Termo | Significado |
| --- | --- |
| **ADR** | Architecture Decision Record: registro de uma decisão de arquitetura, com contexto e consequências |
| **BFF** | Backend for Frontend: a API feita sob medida para o console (`velqi-api`) |
| **BVA** | Boundary Value Analysis: testar nos limites (mínimo, máximo, logo antes e logo depois) |
| **Catálogo** | Conjunto versionado de skills distribuído como pacote npm |
| **Cenário** | Caso de teste estruturado: ID, passos, resultado esperado, técnica, prioridade e critério de aceite |
| **Evidência** | Prova da execução (log, print, relatório) anexada à execução e ao PR |
| **Execução** | Uma rodada da Velqi, do pedido ao PR, com estados auditáveis |
| **Gate (GP1–GP3)** | Verificação de processo que precisa passar antes de avançar (reflexão, plano completo, plano adaptativo) |
| **Golden set** | Lista fixa de pedidos com o plano de skills esperado, usada como teste de regressão do roteamento |
| **Guardrail (G1–G6)** | Regra que a agente nunca quebra (ex.: G1, nunca inventar massa) |
| **HITL** | Human-in-the-loop: a pausa em que uma pessoa aprova os cenários antes de virarem código |
| **L1–L4** | Níveis de carga de uma skill, do conceito (L1) aos templates de código (L4) |
| **Massa** | Dados de teste reais por ambiente (CPF, agência, conta) vindos de `test-data/` ou do cofre |
| **MCP** | Model Context Protocol: o jeito padronizado do agente falar com sistemas externos |
| **Partição de equivalência** | Dividir as entradas em grupos que se comportam igual e testar um de cada |
| **Roteador** | Parte que transforma o pedido em intenção, e a intenção em um plano de skills |
| **Runner** | Onde o agente de código executa (máquina do dev ou servidor central) |
| **Skill** | Pasta com `SKILL.md` e materiais que ensinam o agente a fazer uma tarefa |
| **SSE** | Server-Sent Events: canal de tempo real do servidor para o console |
| **Tabela de decisão** | Técnica que cruza condições combinadas para cobrir todas as regras |
| **velqi-core** | Skill sempre ativa com a persona, os guardrails e os gates |

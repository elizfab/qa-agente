# 0001. Registrar decisões de arquitetura com ADRs

- **Status:** Aceita
- **Data:** 2026-10-10
- **Decisores:** Liza (Elizabete Fabri)

## Contexto

O projeto tem várias decisões em aberto (linguagem da API, onde o agente roda, design system). Sem registro, o motivo de cada escolha se perde, e decisões são rediscutidas sem contexto.

## Decisão

Toda decisão que afete arquitetura, stack, segurança ou contrato entre camadas é registrada como ADR em `docs/arquitetura/adr/`, seguindo o template `0000-template.md`.

## Consequências

- Novas pessoas entendem o porquê das escolhas sem depender de quem decidiu.
- Toda mudança de decisão gera uma nova ADR, o que deixa o histórico explícito.
- O PR que implementa uma decisão deve citar a ADR correspondente.

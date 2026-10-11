# Critérios de aceite do MVP

O MVP está pronto quando uma squad piloto consegue sair de um card do IUClick e chegar a um PR com testes Playwright passando. Isso precisa acontecer sem nenhum guardrail violado e com todas as decisões registradas na auditoria.

## Critérios

- [ ] A partir de um card real, a Velqi gera cenários com técnica e rastreio a cada critério de aceite
- [ ] A execução para em "Aguardando aprovação" e só segue após decisão no console
- [ ] Cenários rejeitados não viram código
- [ ] O código segue o padrão detectado pelo `project-discovery` e os testes passam
- [ ] Uma falha real de teste interrompe o fluxo e gera relatório (nenhum assert enfraquecido)
- [ ] Nenhum commit ou push sem confirmação; nenhum na `main`
- [ ] `velqi_verify_plan` impede "concluído" com pendência
- [ ] A auditoria mostra quem aprovou, quando e qual versão do catálogo
- [ ] O golden set de roteamento tem taxa de acerto de pelo menos 90%
- [ ] O console não tem violações críticas de WCAG 2.2 AA

## Perguntas em aberto

Cada pergunta, quando decidida, vira uma ADR em [`../arquitetura/adr`](../arquitetura/adr/README.md).

| # | Pergunta | ADR |
| --- | --- | --- |
| 1 | Onde o agente roda: na máquina do dev (como hoje) ou em um runner central que o console dispara? | [0005](../arquitetura/adr/0005-onde-o-agente-roda.md) (proposta) |
| 2 | BFF em Go hexagonal (mesmo padrão do Rollout Service) ou NestJS (mesma linguagem do front)? | [0004](../arquitetura/adr/0004-linguagem-do-bff.md) (proposta) |
| 3 | A Velqi é uma evolução do QA Agent NQD (reaproveita skills) ou um produto separado? | — |
| 4 | Seguir o design system corporativo ou manter a identidade própria da Velqi? | — |
| 5 | Qual squad piloto e qual projeto web para o MVP? | — |
| 6 | Retenção dos eventos de auditoria: por quanto tempo e onde? | — |
| 7 | Quem pode aprovar cenários: qualquer QA da squad ou só o QA responsável pelo card? | — |

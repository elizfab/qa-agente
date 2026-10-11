# 0004. Linguagem da API (BFF): Go ou NestJS

- **Status:** Proposta
- **Data:** 2026-10-10
- **Decisores:** a definir

## Contexto

A API guarda execuções, decisões e auditoria, mantém a máquina de estados e transmite eventos via SSE. Ela precisa ser simples de operar no ambiente do banco e fácil de manter pelo time.

## Opções consideradas

| Opção | Prós | Contras |
| --- | --- | --- |
| Go, arquitetura hexagonal | Mesmo padrão do Rollout Service; binário único; ótimo para SSE e concorrência | Duas linguagens no projeto (TS no front, Go no back) |
| NestJS | Mesma linguagem do console; tipos compartilhados entre front e back | Mais dependências; padrão diferente do serviço Go que o time já mantém |

## Decisão

A decidir.

## Consequências

Serão preenchidas quando a decisão for tomada.

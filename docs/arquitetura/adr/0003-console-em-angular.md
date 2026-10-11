# 0003. Console web em Angular

- **Status:** Aceita
- **Data:** 2026-10-10
- **Decisores:** Liza (Elizabete Fabri)

## Contexto

O sistema de referência (QA Agent NQD) não tem interface: tudo acontece no chat do agente. A Velqi precisa de um console para execuções, aprovação HITL, catálogo e auditoria.

## Decisão

O console é uma aplicação **Angular** (versão estável mais recente), com standalone components, signals, o novo control flow (`@if`, `@for`) e lazy loading por rota.

## Consequências

- O time já domina Angular e os MFEs existentes, o que reduz a curva de aprendizado.
- Os signals simplificam o estado que muda em tempo real (status via SSE).
- Os design tokens da [identidade visual](../../design/identidade-visual.md) viram `design-system/tokens.css`.
- Testes: Vitest ou Jest para unidade, Playwright para E2E e axe para acessibilidade (escolha final no scaffold).

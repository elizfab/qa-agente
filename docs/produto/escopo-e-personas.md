# Escopo e personas

O MVP entrega o ciclo completo para **um** tipo de trabalho: automação web E2E com Playwright a partir de um card do IUClick, com HITL e auditoria no console. O resto do catálogo entra por fases.

> **Por que começar estreito:** é como abrir um restaurante com um cardápio de cinco pratos. Você descobre onde a cozinha trava antes de oferecer cinquenta.

## Fases

| Fase | Entra | Fica de fora |
| --- | --- | --- |
| MVP | velqi-core, guardrails G1–G6, GP1–GP3, roteamento, project-discovery, test-design, web-automation, iuclick, test-data-management, plan-tracker, console (execuções, HITL, catálogo, auditoria) | Mobile, performance, chaos, segurança |
| Fase 2 | repository-analysis, figma-flow-analysis, api-testing, backend-testing, bdd-testing, hopper, sonar-quality, ci-cd-quality-gates | Mobile, chaos |
| Fase 3 | mobile (appium, assert-mobile, superapp, preflight), performance, chaos, security, accessibility, mutation, gestão de experiência, métricas de ROI | — |

## Personas

| Persona | O que quer da Velqi | Tela principal |
| --- | --- | --- |
| QA / Quality Engineer | Gerar cenários e automação a partir de um card, revisar antes do código | Nova execução, Aprovação HITL |
| Dev de squad | Testes junto do PR, sem virar especialista em QA | Execuções, diff do código gerado |
| Tech lead / Gerente de qualidade | Ver cobertura, gates e guardrails da squad | Painel, Auditoria |
| Mantenedor do catálogo (Eng Foundation) | Publicar, versionar e testar skills | Catálogo de skills |

# Sistema de referência: QA Agent NQD

O QA Agent NQD 2.4.1 não é um servidor rodando: é um **catálogo de 30 skills** distribuído como pacote npm. Ele transforma um agente de IA de código em um Quality Engineer sênior que implementa de verdade.

Este documento resume o que foi lido nas imagens do sistema e serve de base para os requisitos da Velqi.

## Como ele chega ao projeto

A CLI `qa-agent` guarda o pacote em cache (`~/.qa-agent/cache/qa-agent-nqd/<versão>/`) e copia as skills para a pasta do agente alvo:

`.claude/skills/`, `.cursor/skills/`, `.github/skills/`, `.windsurf/skills/`, `.agents/skills/`, `.gemini/skills/`.

## Anatomia de uma skill

Caminho: `(categoria)/<skill>/`

| Item | Obrigatório | Papel |
| --- | --- | --- |
| `SKILL.md` | Sim | Frontmatter YAML + corpo (até 600 linhas) |
| `references/` | Não | Material de apoio carregado sob demanda |
| `templates/` | Não | Artefatos prontos (md, ts, py, yml, feature) |
| `scripts/` | Não | Automação, servidores MCP, setup |
| `sdk/` | Não | SDK Python (hopper, iuclick) |
| `tools/` | Não | Sub-skills aninhadas (ex.: playwright-skill) |

O frontmatter define:

- `name`
- `description`, com frases-gatilho e escopo negativo
- `metadata.triggers`
- `metadata.intent`
- `loadLevels`, de L1 a L4

> **Analogia:** os níveis funcionam como o zoom de um mapa. L1 é a visão do país (só o `SKILL.md`, para entender) e L4 é a rua com número (templates e código, para implementar). Isso economiza contexto do modelo.

## Fluxo de execução

1. `qa-core` (persona + núcleo de qualidade) está sempre ativo.
2. O gatilho por palavra-chave vira intenção. O roteamento (`_index`, `_routing`, `_examples`) transforma a intenção em um plano de skills.
3. Skills de domínio são carregadas sob demanda, no nível certo (L1 → L4).
4. Test Design → cenários → **HITL** (pausa humana) → implementação → CI/Quality Gates.

## Guardrails sempre ativos

| ID | Regra |
| --- | --- |
| G1 | Massa de teste nunca é hipotética (CPF, agência e conta vêm do usuário ou de `test-data/`) |
| G2 | MCP First: sistemas externos só via tools MCP |
| G3 | Human-in-the-loop: após derivar cenários, para e aguarda confirmação |
| G4 | Falha em execução = parar e reportar; nunca mascarar bug |
| G5 | Git: nunca commit/push sem confirmação; nunca na `main` |
| G6 | Instalar ou atualizar skills exige confirmação |

## Gates de processo

| Gate | O que garante |
| --- | --- |
| GP1 | Qualidade do que é entregue, com reflexão pré-entrega |
| GP2 | Plano multi-step completo, materializado pela tool `nqd_verify_plan` |
| GP3 | Plano adaptativo a mudanças de requisito |

## Catálogo por categoria

| Categoria | Qtd. | Skills |
| --- | --- | --- |
| testing | 6 | test-design, test-planning, bug-reporting, mutation-testing, accessibility-testing, security-testing |
| automation | 9 | web-automation (playwright), mobile-automation, appium, assert-mobile (Robot), api-testing, backend-testing (assert-back, hypothesis, junit5, pytest), bdd-testing (behave), performance-testing (p4all, k6), autonomous-e2e |
| platform | 12 | project-discovery, repository-analysis, figma-flow-analysis, ci-cd-quality-gates, taac-integration, test-data-management, hopper, iuclick, health-check, chaos-testing, mobile-infra-preflight, plan-tracker |
| domain | 3 (+1) | qa-core, sonar-quality, gestao-experiencia, automation-superapp-mobile (+ enrichment-kb) |

**Workflows ponta a ponta (`sdds/`):**

- implement-new-api
- implement-new-e2e-web
- implement-new-mobile
- review-spec-full
- repo-analysis-full
- autonomous-e2e-hopper

## Scripts e servidores MCP

| Script | O que expõe |
| --- | --- |
| `hopper_mcp_server.py` | 20 tools `hopper_*` (workspaces, pastas, cenários, planos, evidências, squads) |
| `iuclick_mcp_server.py` | 6 tools `iuclick_*` (cards, detalhes, comentários, sessão) |
| `plan_tracker_mcp.py` | `nqd_verify_plan`, que bloqueia "concluído" com itens pendentes |
| `sonarqube_mcp_server.py` | `sonar_overview`, `sonar_issues`, `sonar_hotspots`, `sonar_metrics` |
| `start-github-mcp.sh` / `proxy-preload.js` | GitHub MCP com proxy corporativo e CA |
| `setup/start-figma-mcp.sh` | Figma MCP com validação de token |
| `setup-project.sh` | Scaffold Playwright (`e2e/{pages,fixtures,helpers,specs}`) |
| `preflight_check.py`, `hcheck.py` | Checagem de device/Appium e de serviços UP/DOWN |
| `manage-skills.js`, `install-qa-agent-cli.js` | Wrapper da CLI e fallback de instalação via Artifactory |

## Autenticação

| Sistema | Método | Onde fica o token |
| --- | --- | --- |
| Hopper | OAuth2 + PKCE (Azure AD) | `~/.hopper` |
| IUClick | SSO SAML2 (cookies ServiceNow + `X-UserToken`) | `~/.iuclick` |

Os dois abrem um navegador embarcado (pywebview) para o login.

## O que falta no sistema de referência

Esta é a oportunidade da Velqi. Hoje não existe uma interface para:

- ver execuções;
- aprovar cenários no HITL;
- auditar guardrails e gates;
- acompanhar a saúde do catálogo.

Tudo acontece dentro da janela de chat do agente.

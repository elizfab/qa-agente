# Organização do repositório

Hoje o repositório tem só documentação e configuração. Quando o código chegar, a ideia é um **monorepo**: catálogo, API e console versionados juntos. Assim, uma mudança de contrato entre as partes sai em um único PR.

## Estrutura atual

```text
qa-agente/
├── .github/
│   ├── workflows/           CI (docs, título de PR)
│   ├── ISSUE_TEMPLATE/      modelos de issue (feature, bug, decisão)
│   ├── pull_request_template.md
│   ├── CODEOWNERS
│   └── dependabot.yml
├── docs/
│   ├── produto/             o quê e por quê
│   ├── arquitetura/         como funciona + ADRs
│   ├── design/              identidade visual, protótipo, assets
│   ├── prompts/             prompts de criação, implementação e validação
│   ├── implementacao/       roadmap, organização, glossário
│   └── arquivo/             materiais antigos, substituídos (somente leitura)
├── CONTRIBUTING.md
└── README.md
```

## Estrutura planejada

As pastas abaixo **ainda não existem**. Cada uma é criada no PR que a implementa.

```text
qa-agente/
├── skills/                  catálogo (Fase 1)
│   ├── _index.md
│   ├── _routing.md
│   ├── _examples/
│   ├── (domain)/velqi-core/
│   ├── (testing)/test-design/
│   ├── (automation)/web-automation/
│   └── (platform)/{project-discovery,iuclick,test-data-management,plan-tracker}/
├── cli/                     CLI `velqi` + validador de skills (Fase 1)
├── api/                     velqi-api, BFF (Fase 2)
└── console/                 velqi-console, Angular (Fase 3)
```

## CI planejado por pasta

Cada workflow só roda quando a sua pasta muda (filtro `paths`), para o CI continuar rápido.

| Pasta | Workflow | Verifica |
| --- | --- | --- |
| `docs/**`, `*.md` | `docs.yml` (já existe) | Markdown lint e links quebrados |
| `skills/**` | `skills.yml` | Validador de frontmatter, limite de 600 linhas, gatilhos duplicados, golden set de roteamento |
| `cli/**` | `cli.yml` | Lint, testes, build do pacote |
| `api/**` | `api.yml` | Lint, testes, cobertura de 80%, build da imagem |
| `console/**` | `console.yml` | Lint, testes unitários, E2E Playwright, axe, cobertura de 80%, build |

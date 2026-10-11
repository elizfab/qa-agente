# Crivo — Agente de QA: Requisitos e Prompts

Oct 9, 2026 · @Liza

## Visão geral

O **Crivo** é um agente de Quality Engineering que analisa specs, designs e repositórios, deriva cenários de teste, pausa para aprovação humana e só então escreve o código de automação no projeto. Ele herda o modelo do QA Agent NQD (catálogo de skills + guardrails) e acrescenta um **console web em Angular** para dar visibilidade, aprovação e auditoria ao que o agente faz.

O nome vem da expressão "passar pelo crivo": examinar com rigor e deixar passar só o que tem qualidade.

**Analogia:** pense no QA Agent NQD como um livro de receitas muito bom (as skills) que qualquer cozinheiro de IA consegue ler. O Crivo é a cozinha inteira: o mesmo livro de receitas, mais um painel na parede mostrando qual prato está sendo feito, em que etapa está e um botão para o chef humano aprovar antes de ir para o forno.

**Premissas adotadas (ajustar conforme validarmos):**

- Público inicial: squads de engenharia do Itaú (QAs, devs, tech leads).
- O motor do agente continua sendo um agente de codificação (Claude Code, Copilot, Cursor) lendo skills; o Crivo não substitui esse motor, ele o orquestra e expõe.
- O console é Angular (standalone components + signals) e conversa com um backend (BFF) que guarda execuções, aprovações e auditoria.
- Integrações externas sempre via MCP (Hopper, IUClick, SonarQube, Figma, GitHub), como no sistema de referência.

## O que o QA Agent NQD faz (leitura das imagens)

O QA Agent NQD 2.4.1 não é um servidor rodando: é um **catálogo de 30 skills** distribuído como pacote npm, que transforma um agente de IA de código em um Quality Engineer sênior que implementa de verdade.

**Como ele chega ao projeto.** A CLI `qa-agent` guarda o pacote em cache (`~/.qa-agent/cache/qa-agent-nqd/<versão>/`) e copia as skills para a pasta do agente alvo: `.claude/skills/`, `.cursor/skills/`, `.github/skills/`, `.windsurf/skills/`, `.agents/skills/`, `.gemini/skills/`.

**Anatomia de uma skill** — `(categoria)/<skill>/`:

| Item | Obrigatório | Papel |
| --- | --- | --- |
| `SKILL.md` | Sim | Frontmatter YAML + corpo (até 600 linhas) |
| `references/` | Não | Material de apoio carregado sob demanda |
| `templates/` | Não | Artefatos prontos (md, ts, py, yml, feature) |
| `scripts/` | Não | Automação, servidores MCP, setup |
| `sdk/` | Não | SDK Python (hopper, iuclick) |
| `tools/` | Não | Sub-skills aninhadas (ex.: playwright-skill) |

O frontmatter define `name`, `description` (com frases-gatilho e escopo negativo), `metadata.triggers`, `metadata.intent` e `loadLevels` de L1 a L4. **Analogia:** os níveis funcionam como um zoom de mapa — L1 é a visão do país (só o SKILL.md, para entender), L4 é a rua com número (templates e código, para implementar). Isso economiza contexto do modelo.

**Fluxo de execução:**

1. `qa-core` (persona + núcleo de qualidade) está sempre ativo.
2. Gatilho por palavra-chave vira intenção; o roteamento (`_index`, `_routing`, `_examples`) transforma a intenção em um plano de skills.
3. Skills de domínio são carregadas sob demanda no nível certo (L1→L4).
4. Test Design → cenários → **HITL** (pausa humana) → implementação → CI/Quality Gates.

**Guardrails sempre ativos:**

| ID | Regra |
| --- | --- |
| G1 | Massa de teste nunca é hipotética (CPF, agência, conta vêm do usuário ou de `test-data/`) |
| G2 | MCP First: sistemas externos só via tools MCP |
| G3 | Human-in-the-loop: após derivar cenários, para e aguarda confirmação |
| G4 | Falha em execução = parar e reportar; nunca mascarar bug |
| G5 | Git: nunca commit/push sem confirmação; nunca na `main` |
| G6 | Instalar/atualizar skills exige confirmação |

**Gates de processo:** GP1 (qualidade do entregue, com reflexão pré-entrega), GP2 (plano multi-step completo, materializado pela tool `nqd_verify_plan`), GP3 (plano adaptativo a mudanças).

**Catálogo por categoria:**

| Categoria | Qtd. | Skills |
| --- | --- | --- |
| testing | 6 | test-design, test-planning, bug-reporting, mutation-testing, accessibility-testing, security-testing |
| automation | 9 | web-automation (playwright), mobile-automation, appium, assert-mobile (Robot), api-testing, backend-testing (assert-back, hypothesis, junit5, pytest), bdd-testing (behave), performance-testing (p4all, k6), autonomous-e2e |
| platform | 12 | project-discovery, repository-analysis, figma-flow-analysis, ci-cd-quality-gates, taac-integration, test-data-management, hopper, iuclick, health-check, chaos-testing, mobile-infra-preflight, plan-tracker |
| domain | 3 (+1) | qa-core, sonar-quality, gestao-experiencia, automation-superapp-mobile (+ enrichment-kb) |

**Workflows ponta a ponta (`sdds/`):** implement-new-api, implement-new-e2e-web, implement-new-mobile, review-spec-full, repo-analysis-full, autonomous-e2e-hopper.

**Scripts e servidores MCP:**

| Script | O que expõe |
| --- | --- |
| hopper\_mcp\_server.py | 20 tools `hopper_*` (workspaces, pastas, cenários, planos, evidências, squads) |
| iuclick\_mcp\_server.py | 6 tools `iuclick_*` (cards, detalhes, comentários, sessão) |
| plan\_tracker\_mcp.py | `nqd_verify_plan` — bloqueia "concluído" com itens pendentes |
| sonarqube\_mcp\_server.py | `sonar_overview`, `sonar_issues`, `sonar_hotspots`, `sonar_metrics` |
| start-github-mcp.sh / proxy-preload.js | GitHub MCP com proxy corporativo e CA |
| setup/start-figma-mcp.sh | Figma MCP com validação de token |
| setup-project.sh | Scaffold Playwright (`e2e/{pages,fixtures,helpers,specs}`) |
| preflight\_check.py, hcheck.py | Checagem de device/Appium e de serviços UP/DOWN |
| manage-skills.js, install-qa-agent-cli.js | Wrapper da CLI e fallback de instalação via Artifactory |

**Autenticação:** Hopper usa OAuth2 + PKCE (Azure AD, token em `~/.hopper`); IUClick usa SSO SAML2 (cookies ServiceNow + `X-UserToken` em `~/.iuclick`); ambos abrem navegador embarcado (pywebview).

**O que falta no sistema de referência** (e é a oportunidade do Crivo): não há uma interface para ver execuções, aprovar cenários no HITL, auditar guardrails e gates, nem um painel de saúde do catálogo. Tudo acontece dentro da janela de chat do agente.

## Escopo e personas

O MVP entrega o ciclo completo para **um** tipo de trabalho — automação web E2E com Playwright a partir de um card do IUClick — com HITL e auditoria no console. O resto do catálogo entra por fases.

**Por que começar estreito:** é como abrir um restaurante com um cardápio de cinco pratos. Você aprende onde a cozinha trava antes de oferecer cinquenta.

| Fase | Entra | Fica de fora |
| --- | --- | --- |
| MVP | crivo-core, guardrails G1–G6, GP1–GP3, roteamento, project-discovery, test-design, web-automation, iuclick, test-data-management, plan-tracker, console (execuções, HITL, catálogo, auditoria) | Mobile, performance, chaos, segurança |
| Fase 2 | repository-analysis, figma-flow-analysis, api-testing, backend-testing, bdd-testing, hopper, sonar-quality, ci-cd-quality-gates | Mobile, chaos |
| Fase 3 | mobile (appium, assert-mobile, superapp, preflight), performance, chaos, security, accessibility, mutation, gestão de experiência, métricas de ROI | — |

**Personas:**

| Persona | O que quer do Crivo | Tela principal |
| --- | --- | --- |
| QA / Quality Engineer | Gerar cenários e automação a partir de um card, revisar antes do código | Nova execução, Aprovação HITL |
| Dev de squad | Testes junto do PR sem virar especialista em QA | Execuções, diff do código gerado |
| Tech lead / Gerente de qualidade | Ver cobertura, gates e guardrails da squad | Painel, Auditoria |
| Mantenedor do catálogo (Eng Foundation) | Publicar, versionar e testar skills | Catálogo de skills |

## Requisitos funcionais

São 46 requisitos em 8 grupos; prioridade M = MVP, F2/F3 = fases seguintes.

**RF-A. Catálogo de skills**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-A01 | Cada skill segue a anatomia `SKILL.md` + `references/` + `templates/` + `scripts/` + `sdk/` + `tools/` | M |
| RF-A02 | O frontmatter tem `name`, `description` (gatilhos + escopo negativo), `metadata.triggers`, `metadata.intent`, `loadLevels` L1–L4 | M |
| RF-A03 | Um validador rejeita skill com frontmatter inválido, SKILL.md acima de 600 linhas ou gatilho duplicado com outra skill | M |
| RF-A04 | O catálogo é versionado (semver) e publicado como pacote npm no Artifactory | M |
| RF-A05 | A CLI `crivo` instala, atualiza, lista e mostra status das skills nas pastas de cada agente (.claude, .cursor, .github, .windsurf, .agents, .gemini) | M |
| RF-A06 | Instalar ou atualizar skills pede confirmação explícita (G6) | M |
| RF-A07 | O console lista as skills por categoria, com versão, gatilhos, nível de carga e dependências | M |
| RF-A08 | Workflows (`flows/`) encadeiam skills em ordem, com nível de carga por etapa | F2 |

**RF-B. Roteamento e carregamento de contexto**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-B01 | `crivo-core` (persona + guardrails + gates) é carregado em toda sessão | M |
| RF-B02 | O roteador transforma o pedido em intenção e a intenção em um plano de skills, usando `_index`, `_routing` e `_examples` | M |
| RF-B03 | O agente carrega L1 para entender e só sobe para L3/L4 ao implementar | M |
| RF-B04 | Quando a intenção é ambígua, o agente pergunta antes de escolher a skill | M |
| RF-B05 | O plano de skills escolhido é registrado na execução e visível no console | M |

**RF-C. Fluxo de execução**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-C01 | `project-discovery` roda obrigatoriamente antes de implementar (linguagem, framework, estrutura, CI) | M |
| RF-C02 | Test Design precede implementação: BVA, partição de equivalência, pairwise, tabela de decisão | M |
| RF-C03 | Cenários saem em formato estruturado (ID, título, pré-condição, passos, resultado esperado, técnica, prioridade, rastreio ao critério de aceite) | M |
| RF-C04 | O código gerado respeita o padrão do projeto (POM, seletores resilientes, waits explícitos) | M |
| RF-C05 | O agente executa os testes gerados e anexa o resultado à execução | M |
| RF-C06 | Cada execução tem estados: Descoberta → Design → Aguardando aprovação → Implementando → Validando → Concluída / Falhou / Cancelada | M |

**RF-D. Human-in-the-loop (HITL)**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-D01 | Após derivar cenários, a execução para em "Aguardando aprovação" (G3) | M |
| RF-D02 | No console, o aprovador marca cada cenário como aprovado, rejeitado ou editado, com comentário | M |
| RF-D03 | Só cenários aprovados seguem para implementação | M |
| RF-D04 | Commit e push pedem confirmação separada e nunca vão para `main` (G5) | M |
| RF-D05 | Notificação (Teams/e-mail) quando uma execução aguarda aprovação | F2 |

**RF-E. Guardrails e gates**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-E01 | G1: o agente recusa inventar massa; busca em `test-data/` ou pede ao usuário | M |
| RF-E02 | G2: chamadas a sistemas externos só via tools MCP registradas | M |
| RF-E03 | G4: falha na execução para o fluxo e gera relatório; o validador bloqueia `waitForTimeout` e asserts enfraquecidos | M |
| RF-E04 | GP1: antes de entregar, checklist de reflexão (riscos, evidência, cobertura, código válido, massa real) | M |
| RF-E05 | GP2: `crivo_verify_plan` bloqueia "concluído" se houver item pendente | M |
| RF-E06 | GP3: mudança de requisito no meio gera nova versão do plano, com diff | F2 |
| RF-E07 | Toda violação ou bloqueio de guardrail vira evento auditável | M |

**RF-F. Integrações (MCP)**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-F01 | IUClick: listar/buscar cards, ler spec e critérios de aceite, comentar | M |
| RF-F02 | GitHub: ler repositório, abrir branch e PR | M |
| RF-F03 | Hopper (IUQuali): publicar cenários, planos de execução e evidências | F2 |
| RF-F04 | SonarQube: overview, issues, hotspots, métricas | F2 |
| RF-F05 | Figma: ler fluxo e gerar mapa de telas | F2 |
| RF-F06 | Todos os servidores MCP funcionam atrás de proxy corporativo com CA bundle | M |

**RF-G. Massa de teste**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-G01 | Massa resolvida por ambiente (DEV, HOM) a partir de `test-data/` ou cofre | M |
| RF-G02 | Dados sensíveis (CPF, conta) mascarados no console e nos logs | M |

**RF-H. Console Angular**

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-H01 | Painel: execuções ativas, aguardando aprovação, taxa de aprovação de cenários, gates | M |
| RF-H02 | Nova execução: escolher fonte (card IUClick, repo, Figma, texto livre), projeto e tipo de teste | M |
| RF-H03 | Detalhe da execução: linha do tempo de etapas, plano de skills, logs, código gerado (diff) | M |
| RF-H04 | Tela de aprovação HITL por cenário | M |
| RF-H05 | Catálogo de skills navegável | M |
| RF-H06 | Auditoria: eventos de guardrail/gate filtráveis por squad, projeto, período | M |
| RF-H07 | Configurações: conexões MCP e status (conectado, expirado) | F2 |

## Requisitos não funcionais

Os não funcionais definem se o Crivo pode rodar dentro do ambiente do banco: segurança e rastreabilidade pesam mais que velocidade.

| ID | Categoria | Requisito |
| --- | --- | --- |
| RNF-01 | Autenticação | Login no console via SSO Azure AD (OIDC + PKCE); MCPs reaproveitam OAuth2/SAML2 como no sistema de referência |
| RNF-02 | Autorização | Perfis: Executor, Aprovador, Admin do catálogo, Leitor; aprovar exige perfil Aprovador da squad |
| RNF-03 | Segredos | Tokens nunca em repositório nem no front; `.env` sempre no `.gitignore`; cofre no backend |
| RNF-04 | Dados sensíveis | Massa (CPF, agência, conta) mascarada em tela e logs; LGPD |
| RNF-05 | Rede | Funciona atrás de proxy corporativo, CA bundle e VPN; pacotes só do Artifactory |
| RNF-06 | Auditoria | Todo evento (aprovação, guardrail, gate, commit) guardado com quem, quando, execução e hash do artefato; retenção mínima a definir |
| RNF-07 | Observabilidade | Logs estruturados, traces por execução e RUM no console (Datadog) |
| RNF-08 | Desempenho | Console carrega em até 2 s (LCP) em rede corporativa; status da execução atualiza em tempo real (SSE ou WebSocket) |
| RNF-09 | Acessibilidade | Console em conformidade WCAG 2.2 AA (contraste, teclado, leitor de tela) |
| RNF-10 | Idioma | Interface em pt-BR; textos externalizados para i18n |
| RNF-11 | Portabilidade | Skills funcionam em Claude Code, Cursor, Copilot, Windsurf, Gemini sem alteração |
| RNF-12 | Qualidade do próprio Crivo | Cobertura mínima de 80% no console e no BFF; o próprio Crivo passa pelos seus quality gates (Sonar) |
| RNF-13 | Determinismo | Scripts (validador, CLI, verify\_plan) têm saída determinística e `--json` com exit code 0/1 |
| RNF-14 | Versionamento | Cada execução registra a versão do catálogo usada, para reproduzir resultados |

## Arquitetura e stack

O console fala só com a API; a API dispara o agente e recebe seus eventos; o agente fala com sistemas externos só via MCP e escreve código só no repositório.

&#91;embedded content: arquitetura do Crivo · 4 camadas\]

**Analogia:** o console é a torre de controle, a API é o rádio, o runner é o piloto e os MCPs são os instrumentos. O piloto voa, mas pede autorização à torre antes de pousar (HITL) e tudo fica gravado na caixa-preta (auditoria).

| Camada | Tecnologia proposta | Por quê |
| --- | --- | --- |
| Console | Angular (standalone, signals, control flow novo), Angular CDK | Padrão do time; signals simplificam o estado em tempo real |
| Tempo real | SSE | Fluxo é só servidor → cliente; mais simples que WebSocket atrás de proxy |
| API (BFF) | Go hexagonal ou NestJS (decidir) | Go reaproveita o padrão do Rollout Service; NestJS unifica linguagem |
| Banco | Postgres | Execuções e auditoria são relacionais e precisam de histórico |
| Runner | Claude Code / Copilot em modo headless com as skills | Mantém o modelo do NQD: o motor é o agente de código |
| MCP | Python + FastMCP + httpx | Igual ao sistema de referência; reaproveita SDKs hopper e iuclick |
| CLI e catálogo | Node, pacote npm no Artifactory | Mesma distribuição do `qa-agent` |
| Observabilidade | Datadog (logs, APM, RUM) | Já usado nos MFEs do time |

**Estrutura do console Angular:**

```text
crivo-console/src/app/
  core/            auth (OIDC), interceptors, guards por perfil, sse.service
  design-system/   tokens.css, botão, badge de status, card, tabela
  shared/          pipes (mascarar CPF/conta), componentes genéricos
  features/
    painel/
    execucoes/     lista, detalhe (linha do tempo, logs, diff)
    aprovacao/     HITL por cenário
    catalogo/      skills por categoria, detalhe da skill
    auditoria/
    configuracoes/ conexões MCP
```

## Identidade: nome, logo e cores

Proposta: **Crivo**, com uma peneira em forma de "C" como símbolo e índigo como cor principal. O protótipo visual mostra tudo aplicado.

**Nome.** Curto, em português, fácil de virar comando (`crivo run`, `crivo-core`) e com significado ligado a qualidade. Alternativas, se quiser comparar:

| Nome | Ideia | Comando |
| --- | --- | --- |
| Crivo (recomendado) | "Passar pelo crivo": examinar com rigor | `crivo` |
| Lince | Olhar afiado que enxerga o defeito | `lince` |
| Aferi | De "aferir": medir e conferir | `aferi` |
| Sentinela | Guarda que nunca dorme (guardrails) | `sentinela` |

**Logo.** Um quadrado arredondado índigo com pontos dispostos em "C", como os furos de uma peneira; o último ponto vira um check menta. Leitura: muita coisa entra, só o que está certo passa. Funciona em 16 px (favicon) porque é só forma, sem texto.

**Paleta.** Cada cor de status tem um significado fixo no fluxo do agente, para a pessoa entender o estado de uma execução só pela cor.

| Token | Hex | Uso |
| --- | --- | --- |
| `--crivo-primary` | #4B3FD1 | Marca, botões principais, links |
| `--crivo-primary-strong` | #3A2FB0 | Hover, foco |
| `--crivo-primary-soft` | #EEEDFD | Fundos de destaque, chips |
| `--crivo-ink` | #12131F | Texto principal, tema escuro |
| `--crivo-muted` | #5B6072 | Texto secundário |
| `--crivo-surface` | #F7F7FB | Fundo da aplicação |
| `--crivo-pass` | #12B886 | Aprovado, teste passou, gate verde |
| `--crivo-hitl` | #F2A20C | Aguardando decisão humana |
| `--crivo-fail` | #E5484D | Falhou, guardrail bloqueou |

**Tipografia:** IBM Plex Sans para interface, IBM Plex Mono para código, logs e IDs de cenário (família única, tom técnico).

**Por que não laranja:** o laranja já é a cor da marca do banco; usar índigo deixa claro que o Crivo é uma ferramenta interna de engenharia, sem competir com a marca. Se a decisão for alinhar ao design system corporativo, troca-se só o `--crivo-primary`.

## Prompts

São 10 prompts prontos para colar no agente de código, em três famílias: **criação** (o que o agente é), **implementação** (o sistema em volta) e **validação** (provar que funciona). Troque o que está entre `<>`.

**Por que separar assim:** é como montar uma equipe. Primeiro você define quem a pessoa é (criação), depois dá a mesa e as ferramentas (implementação), e por fim faz a avaliação de desempenho (validação).

### Criação

**P1 — Persona e núcleo (`crivo-core/SKILL.md`)**

```markdown
Crie o arquivo skills/(domain)/crivo-core/SKILL.md.

Frontmatter YAML:
- name: crivo-core
- description: persona e núcleo de governança do Crivo, sempre ativo; NÃO usar para tarefas que não sejam de qualidade de software.
- metadata.intent: governance
- loadLevels: L1 (persona + guardrails), L2 (gates e checklists)

Corpo (máx. 600 linhas), seções:
1. Persona: Quality Engineer sênior que implementa de verdade; responde em pt-BR; nunca promete o que não executou.
2. Modelo operacional: (a) carregar contexto no nível mínimo (L1 para entender, L3+ para implementar); (b) Test Design antes de código; (c) reflexão pré-entrega.
3. Guardrails G1–G6 como regras numeradas, cada uma com: regra, por quê, o que fazer em caso de conflito, exemplo de recusa.
   G1 massa nunca hipotética; G2 MCP first; G3 HITL após cenários; G4 falha = parar e reportar, proibido waitForTimeout e assert enfraquecido; G5 git sem confirmação nunca, nunca na main; G6 instalar/atualizar skill exige confirmação.
4. Gates GP1 (checklist: riscos, evidência, cobertura, código válido, massa real), GP2 (chamar crivo_verify_plan antes de dizer "concluído"), GP3 (mudança de requisito = nova versão do plano com diff).
5. Formato de saída de cenários: tabela com ID, título, pré-condição, passos, resultado esperado, técnica, prioridade, critério de aceite rastreado.
6. Estados da execução e quando emitir cada evento para o console (descoberta, design, aguardando_aprovacao, implementando, validando, concluida, falhou).

Não invente integrações além de: iuclick_*, hopper_*, sonar_*, figma_*, github_*.
```

**P2 — Nova skill de domínio**

```markdown
Crie a skill <nome> na categoria (<testing|automation|platform|domain>) seguindo a anatomia:
SKILL.md, references/, templates/, scripts/ (se houver automação), tools/ (se houver sub-skills).

Contexto da skill: <o que ela faz, ferramenta, padrão da empresa>.

Regras:
- description com 3 a 6 frases-gatilho reais que um QA digitaria e um escopo negativo ("não usar para...").
- metadata.triggers sem colisão com as skills existentes em skills/_index.md (leia antes).
- loadLevels: L1 = conceito e quando usar; L2 = passo a passo; L3 = convenções de código; L4 = templates.
- Referências longas vão para references/, nunca no SKILL.md.
- Inclua 2 exemplos de entrada → saída esperada em skills/_examples/<nome>.md.
- Atualize skills/_index.md e skills/_routing.md com a nova intenção.
Ao final, rode `crivo validate skills/(<categoria>)/<nome>` e mostre a saída.
```

**P3 — Roteamento**

```markdown
Crie skills/_routing.md e skills/_index.md.
_index: tabela com skill, categoria, intenção, gatilhos, nível padrão.
_routing: para cada intenção (ex.: "automatizar fluxo web a partir de card"), a sequência de skills e o nível de carga de cada uma.
Regra de desempate: se dois roteamentos casarem, pergunte ao usuário em vez de escolher.
Crie skills/_examples/routing-golden.jsonl com 30 pedidos reais e o plano esperado, para teste de regressão.
```

### Implementação

**P4 — Scaffold do console Angular**

```markdown
Crie o projeto crivo-console em Angular (versão estável mais recente), com:
- standalone components, signals, novo control flow (@if, @for), lazy loading por rota.
- Rotas: /painel, /execucoes, /execucoes/:id, /execucoes/:id/aprovacao, /catalogo, /catalogo/:skill, /auditoria, /configuracoes.
- Estrutura: src/app/{core (auth, http, interceptors), shared (ui, pipes), features/<rota>, design-system}.
- Design tokens em design-system/tokens.css (cores do Crivo: primary #4B3FD1, pass #12B886, hitl #F2A20C, fail #E5484D, ink #12131F, surface #F7F7FB), fontes IBM Plex Sans e IBM Plex Mono, tema claro e escuro.
- Auth: OIDC com Azure AD (PKCE) via biblioteca de mercado, guard por perfil (Executor, Aprovador, Admin, Leitor).
- Estado da execução em tempo real via SSE em /api/execucoes/:id/eventos.
- Testes: Jest ou Vitest para unidade, Playwright para E2E, axe para acessibilidade.
Antes de gerar código, liste a árvore de pastas e espere minha confirmação.
```

**P5 — Tela de aprovação HITL**

```markdown
Implemente features/aprovacao no crivo-console.
Entrada: GET /api/execucoes/:id/cenarios → lista de cenários (id, titulo, tecnica, prioridade, passos[], esperado, criterioAceite).
UI:
- Cabeçalho com card de origem, projeto, versão do catálogo e contagem (n aprovados / n total).
- Lista de cenários; cada item com Aprovar, Rejeitar (comentário obrigatório) e Editar (abre editor dos passos).
- Filtros por técnica e prioridade; ação em lote "aprovar todos de prioridade alta".
- Botão "Liberar implementação" só habilita quando nenhum cenário está pendente.
- Ao liberar: POST /api/execucoes/:id/decisao com o resultado por cenário.
Regras: a11y por teclado (j/k para navegar, a/r para aprovar/rejeitar), status com cor E texto (nunca só cor), dados sensíveis mascarados.
Escreva testes unitários do componente e um E2E do fluxo aprovar → liberar.
```

**P6 — BFF (API)**

```markdown
Crie o crivo-api em <Go com arquitetura hexagonal | NestJS>.
Domínio: Execucao, Etapa, Cenario, Decisao, EventoAuditoria, Skill, VersaoCatalogo.
Portas de saída: repositório (Postgres), fila de eventos, cliente do runner do agente, cofre de segredos.
Endpoints: POST /execucoes, GET /execucoes, GET /execucoes/:id, GET /execucoes/:id/eventos (SSE), GET /execucoes/:id/cenarios, POST /execucoes/:id/decisao, GET /catalogo, GET /auditoria.
Máquina de estados da execução (descoberta → design → aguardando_aprovacao → implementando → validando → concluida | falhou | cancelada) com transições inválidas rejeitadas.
Toda decisão e todo bloqueio de guardrail gravam EventoAuditoria (quem, quando, execução, hash do artefato).
Logs estruturados e traces compatíveis com Datadog.
```

**P7 — Servidor MCP `crivo_verify_plan`**

```markdown
Crie skills/(platform)/plan-tracker/scripts/plan_tracker_mcp.py usando FastMCP (JSON-RPC 2.0 por stdio).
Tool: crivo_verify_plan(plan_path) → lê o checklist markdown do plano, retorna {total, concluidos, pendentes[], pode_concluir: bool}.
Se pendentes > 0, a resposta deve instruir o agente a NÃO declarar a tarefa concluída.
Também emitir o resultado como evento para o crivo-api (POST /execucoes/:id/eventos) quando CRIVO_EXECUCAO_ID existir.
Testes com pytest cobrindo: plano completo, plano com pendência, arquivo ausente, markdown malformado.
Respeitar HTTPS_PROXY e REQUESTS_CA_BUNDLE.
```

### Validação

**P8 — Revisar uma skill**

```markdown
Revise a skill em <caminho> como mantenedor do catálogo do Crivo. Responda em tabela (critério, ok/falha, evidência, correção sugerida):
1. Frontmatter completo e válido (name, description com gatilhos e escopo negativo, triggers, intent, loadLevels).
2. SKILL.md com até 600 linhas; nada que deveria estar em references/.
3. Gatilhos sem colisão com _index.md.
4. Respeita G1–G6 (procure massa inventada, waitForTimeout, commit automático).
5. Templates compilam/rodam no projeto de exemplo.
6. Exemplos em _examples/ cobrem caso feliz e um caso de recusa.
Não altere arquivos; só reporte.
```

**P9 — Validar cenários gerados (GP1)**

```markdown
Você é um revisor independente que NÃO viu a geração. Recebe: o card <id> (critérios de aceite) e a lista de cenários.
Verifique:
- Cada critério de aceite tem ao menos um cenário positivo e um negativo.
- Técnicas aplicadas fazem sentido (BVA em campos numéricos/limites, tabela de decisão em regras combinadas).
- Nenhum dado de massa é hipotético.
- Riscos não cobertos.
Saída: matriz critério × cenário, lacunas, e veredito APROVA / REPROVA com motivo.
```

**P10 — Validar o console**

```markdown
Para o crivo-console:
1. Rode testes unitários e E2E; reporte falhas sem alterar asserts.
2. Rode axe em todas as rotas; liste violações WCAG 2.2 AA.
3. Verifique que nenhum token ou dado sensível aparece em localStorage, logs do console ou bundle.
4. Meça LCP das rotas /painel e /execucoes/:id.
5. Rode o golden set de roteamento (skills/_examples/routing-golden.jsonl) e informe a taxa de acerto.
Entregue um relatório com o que passou, o que falhou e a evidência de cada item.
```

## Critérios de aceite do MVP e perguntas em aberto

O MVP está pronto quando uma squad piloto sai de um card do IUClick até um PR com testes Playwright passando, sem nenhum guardrail violado e com todas as decisões registradas na auditoria.

**Critérios de aceite:**

- [ ] A partir de um card real, o Crivo gera cenários com técnica e rastreio a cada critério de aceite
- [ ] A execução para em "Aguardando aprovação" e só segue após decisão no console
- [ ] Cenários rejeitados não viram código
- [ ] O código segue o padrão detectado pelo project-discovery e os testes rodam verdes
- [ ] Falha real de teste interrompe o fluxo e gera relatório (nenhum assert enfraquecido)
- [ ] Nenhum commit/push sem confirmação; nenhum na `main`
- [ ] `crivo_verify_plan` impede "concluído" com pendência
- [ ] Auditoria mostra quem aprovou, quando e qual versão do catálogo
- [ ] Golden set de roteamento com taxa de acerto de pelo menos 90%
- [ ] Console sem violações WCAG 2.2 AA críticas

**Perguntas em aberto (precisamos decidir juntas):**

- [ ] Onde o agente roda: na máquina do dev (como hoje) ou em um runner central que o console dispara?
- [ ] BFF em Go hexagonal (mesmo padrão do Rollout Service) ou NestJS (mesma linguagem do front)?
- [ ] O Crivo é uma evolução do QA Agent NQD (mesmo time, reaproveita skills) ou um produto separado?
- [ ] Seguir o design system corporativo ou manter identidade própria do Crivo?
- [ ] Qual squad piloto e qual projeto web para o MVP?
- [ ] Retenção dos eventos de auditoria: quanto tempo e onde?
- [ ] Quem pode aprovar cenários: qualquer QA da squad ou só o QA responsável pelo card?

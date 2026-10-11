# Requisitos funcionais

São 47 requisitos em 8 grupos. Prioridade: **M** = MVP; **F2**/**F3** = fases seguintes (ver [escopo](escopo-e-personas.md)).

Cada ID é estável: issues, PRs e testes devem citar o ID (ex.: `RF-D01`) para manter a rastreabilidade.

## RF-A. Catálogo de skills

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-A01 | Cada skill segue a anatomia `SKILL.md` + `references/` + `templates/` + `scripts/` + `sdk/` + `tools/` | M |
| RF-A02 | O frontmatter tem `name`, `description` (gatilhos + escopo negativo), `metadata.triggers`, `metadata.intent` e `loadLevels` L1–L4 | M |
| RF-A03 | Um validador rejeita skill com frontmatter inválido, `SKILL.md` acima de 600 linhas ou gatilho duplicado com outra skill | M |
| RF-A04 | O catálogo é versionado (semver) e publicado como pacote npm no Artifactory | M |
| RF-A05 | A CLI `velqi` instala, atualiza, lista e mostra o status das skills nas pastas de cada agente (.claude, .cursor, .github, .windsurf, .agents, .gemini) | M |
| RF-A06 | Instalar ou atualizar skills pede confirmação explícita (G6) | M |
| RF-A07 | O console lista as skills por categoria, com versão, gatilhos, nível de carga e dependências | M |
| RF-A08 | Workflows (`flows/`) encadeiam skills em ordem, com nível de carga por etapa | F2 |

## RF-B. Roteamento e carregamento de contexto

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-B01 | `velqi-core` (persona + guardrails + gates) é carregado em toda sessão | M |
| RF-B02 | O roteador transforma o pedido em intenção, e a intenção em um plano de skills, usando `_index`, `_routing` e `_examples` | M |
| RF-B03 | O agente carrega L1 para entender e só sobe para L3/L4 ao implementar | M |
| RF-B04 | Quando a intenção é ambígua, o agente pergunta antes de escolher a skill | M |
| RF-B05 | O plano de skills escolhido é registrado na execução e fica visível no console | M |

## RF-C. Fluxo de execução

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-C01 | `project-discovery` roda obrigatoriamente antes de implementar (linguagem, framework, estrutura, CI) | M |
| RF-C02 | Test Design precede a implementação: BVA, partição de equivalência, pairwise, tabela de decisão | M |
| RF-C03 | Cenários saem em formato estruturado (ID, título, pré-condição, passos, resultado esperado, técnica, prioridade, rastreio ao critério de aceite) | M |
| RF-C04 | O código gerado respeita o padrão do projeto (POM, seletores resilientes, waits explícitos) | M |
| RF-C05 | O agente executa os testes gerados e anexa o resultado à execução | M |
| RF-C06 | Cada execução tem estados: Descoberta → Design → Aguardando aprovação → Implementando → Validando → Concluída / Falhou / Cancelada | M |

## RF-D. Human-in-the-loop (HITL)

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-D01 | Após derivar cenários, a execução para em "Aguardando aprovação" (G3) | M |
| RF-D02 | No console, o aprovador marca cada cenário como aprovado, rejeitado ou editado, com comentário | M |
| RF-D03 | Só cenários aprovados seguem para implementação | M |
| RF-D04 | Commit e push pedem confirmação separada e nunca vão para a `main` (G5) | M |
| RF-D05 | Notificação (Teams/e-mail) quando uma execução aguarda aprovação | F2 |

## RF-E. Guardrails e gates

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-E01 | G1: o agente recusa inventar massa; busca em `test-data/` ou pede ao usuário | M |
| RF-E02 | G2: chamadas a sistemas externos só via tools MCP registradas | M |
| RF-E03 | G4: falha na execução para o fluxo e gera relatório; o validador bloqueia `waitForTimeout` e asserts enfraquecidos | M |
| RF-E04 | GP1: antes de entregar, checklist de reflexão (riscos, evidência, cobertura, código válido, massa real) | M |
| RF-E05 | GP2: `velqi_verify_plan` bloqueia "concluído" se houver item pendente | M |
| RF-E06 | GP3: mudança de requisito no meio gera nova versão do plano, com diff | F2 |
| RF-E07 | Toda violação ou bloqueio de guardrail vira evento auditável | M |

## RF-F. Integrações (MCP)

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-F01 | IUClick: listar e buscar cards, ler spec e critérios de aceite, comentar | M |
| RF-F02 | GitHub: ler repositório, abrir branch e PR | M |
| RF-F03 | Hopper (IUQuali): publicar cenários, planos de execução e evidências | F2 |
| RF-F04 | SonarQube: overview, issues, hotspots, métricas | F2 |
| RF-F05 | Figma: ler fluxo e gerar mapa de telas | F2 |
| RF-F06 | Todos os servidores MCP funcionam atrás de proxy corporativo com CA bundle | M |

## RF-G. Massa de teste

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-G01 | Massa resolvida por ambiente (DEV, HOM) a partir de `test-data/` ou de um cofre | M |
| RF-G02 | Dados sensíveis (CPF, conta) mascarados no console e nos logs | M |

## RF-H. Console Angular

| ID | Requisito | Prioridade |
| --- | --- | --- |
| RF-H01 | Painel: execuções ativas, aguardando aprovação, taxa de aprovação de cenários, gates | M |
| RF-H02 | Nova execução: escolher fonte (card IUClick, repo, Figma, texto livre), projeto e tipo de teste | M |
| RF-H03 | Detalhe da execução: linha do tempo de etapas, plano de skills, logs, código gerado (diff) | M |
| RF-H04 | Tela de aprovação HITL por cenário | M |
| RF-H05 | Catálogo de skills navegável | M |
| RF-H06 | Auditoria: eventos de guardrail e gate filtráveis por squad, projeto e período | M |
| RF-H07 | Configurações: conexões MCP e status (conectado, expirado) | F2 |
| RF-H08 | Página "Sobre a Velqi": significado do nome, como ela trabalha e o que nunca faz | M |

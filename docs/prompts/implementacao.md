# Prompts de implementação

Constroem o sistema em volta da agente: console, tela de aprovação, API e servidor MCP.

## P4. Scaffold do console Angular

Requisitos: RF-H01–H08, RNF-01, RNF-02, RNF-09, RNF-15. Decisão: [ADR 0003](../arquitetura/adr/0003-console-em-angular.md).

```markdown
Crie o projeto velqi-console em Angular (versão estável mais recente), com:
- standalone components, signals, novo control flow (@if, @for), lazy loading por rota.
- Rotas: /painel, /execucoes, /execucoes/:id, /execucoes/:id/aprovacao, /catalogo, /catalogo/:skill, /auditoria, /configuracoes, /sobre.
- Estrutura: src/app/{core (auth, http, interceptors), shared (ui, pipes), features/<rota>, design-system}.
- Design tokens em design-system/tokens.css, copiados de docs/design/identidade-visual.md:
  primary #4B3FD1, accent #F75F01, tema escuro (padrão) com base #201F25 e tema claro com base #F7F7FB, pastéis semânticos com texto escuro.
  Fontes IBM Plex Sans e IBM Plex Mono. Tema alternável por atributo data-theme no <html>.
- Auth: OIDC com Azure AD (PKCE) via biblioteca de mercado, guard por perfil (Executor, Aprovador, Admin, Leitor).
- Estado da execução em tempo real via SSE em /api/execucoes/:id/eventos.
- Testes: Vitest ou Jest para unidade, Playwright para E2E, axe para acessibilidade.
Use as imagens em docs/design/prototipo como referência visual.
Antes de gerar código, liste a árvore de pastas e espere minha confirmação.
```

## P5. Tela de aprovação HITL

Requisitos: RF-D01–D03, RF-H04, RF-G02, RNF-09.

```markdown
Implemente features/aprovacao no velqi-console, seguindo docs/design/prototipo/aprovacao.png.
Entrada: GET /api/execucoes/:id/cenarios → lista de cenários (id, titulo, tecnica, prioridade, passos[], esperado, criterioAceite).
UI:
- Cabeçalho com card de origem, projeto, versão do catálogo e contagem (n decididos / n total) com barra de progresso no laranja de destaque.
- Lista de cenários; cada item com Aprovar, Rejeitar (comentário obrigatório) e Editar (abre editor dos passos).
- Filtros por técnica e prioridade; ação em lote "aprovar todos de prioridade alta".
- Botão "Liberar implementação" só habilita quando nenhum cenário está pendente.
- Ao liberar: POST /api/execucoes/:id/decisao com o resultado por cenário.
Regras: a11y por teclado (j/k para navegar, a/r para aprovar/rejeitar), status com cor, ícone E texto (nunca só cor), dados sensíveis mascarados.
Escreva testes unitários do componente e um E2E do fluxo aprovar → liberar.
```

## P6. BFF (API)

Requisitos: RF-C06, RF-E07, RNF-06, RNF-07. Decisão pendente: [ADR 0004](../arquitetura/adr/0004-linguagem-do-bff.md).

```markdown
Crie o velqi-api em <Go com arquitetura hexagonal | NestJS>.
Domínio: Execucao, Etapa, Cenario, Decisao, EventoAuditoria, Skill, VersaoCatalogo.
Portas de saída: repositório (Postgres), fila de eventos, cliente do runner do agente, cofre de segredos.
Endpoints: POST /execucoes, GET /execucoes, GET /execucoes/:id, GET /execucoes/:id/eventos (SSE), GET /execucoes/:id/cenarios, POST /execucoes/:id/decisao, GET /catalogo, GET /auditoria.
Máquina de estados da execução conforme docs/arquitetura/visao-arquitetural.md, com transições inválidas rejeitadas.
Toda decisão e todo bloqueio de guardrail gravam EventoAuditoria (quem, quando, execução, hash do artefato).
Logs estruturados e traces compatíveis com Datadog.
```

## P7. Servidor MCP `velqi_verify_plan`

Requisitos: RF-E05, RNF-13.

```markdown
Crie skills/(platform)/plan-tracker/scripts/plan_tracker_mcp.py usando FastMCP (JSON-RPC 2.0 por stdio).
Tool: velqi_verify_plan(plan_path) → lê o checklist markdown do plano, retorna {total, concluidos, pendentes[], pode_concluir: bool}.
Se pendentes > 0, a resposta deve instruir o agente a NÃO declarar a tarefa concluída.
Também emitir o resultado como evento para o velqi-api (POST /execucoes/:id/eventos) quando VELQI_EXECUCAO_ID existir.
Testes com pytest cobrindo: plano completo, plano com pendência, arquivo ausente, markdown malformado.
Respeitar HTTPS_PROXY e REQUESTS_CA_BUNDLE.
```

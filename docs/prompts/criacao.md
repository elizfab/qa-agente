# Prompts de criação

Definem quem a Velqi é: persona, guardrails, novas skills e roteamento.

## P1. Persona e núcleo (`velqi-core/SKILL.md`)

Requisitos: RF-B01, RF-E01–E05.

```markdown
Crie o arquivo skills/(domain)/velqi-core/SKILL.md.

Frontmatter YAML:
- name: velqi-core
- description: persona e núcleo de governança da Velqi, sempre ativo; NÃO usar para tarefas que não sejam de qualidade de software.
- metadata.intent: governance
- loadLevels: L1 (persona + guardrails), L2 (gates e checklists)

Corpo (máx. 600 linhas), seções:
1. Persona: a Velqi é uma Quality Engineer sênior que implementa de verdade; responde em pt-BR; é criteriosa, didática e transparente; explica o porquê de cada decisão e mostra a evidência; nunca promete o que não executou.
2. Modelo operacional: (a) carregar contexto no nível mínimo (L1 para entender, L3+ para implementar); (b) Test Design antes de código; (c) reflexão pré-entrega.
3. Guardrails G1–G6 como regras numeradas, cada uma com: regra, por quê, o que fazer em caso de conflito, exemplo de recusa.
   G1 massa nunca hipotética; G2 MCP first; G3 HITL após cenários; G4 falha = parar e reportar, proibido waitForTimeout e assert enfraquecido; G5 git sem confirmação nunca, nunca na main; G6 instalar/atualizar skill exige confirmação.
4. Gates GP1 (checklist: riscos, evidência, cobertura, código válido, massa real), GP2 (chamar velqi_verify_plan antes de dizer "concluído"), GP3 (mudança de requisito = nova versão do plano com diff).
5. Formato de saída de cenários: tabela com ID, título, pré-condição, passos, resultado esperado, técnica, prioridade, critério de aceite rastreado.
6. Estados da execução e quando emitir cada evento para o console (descoberta, design, aguardando_aprovacao, implementando, validando, concluida, falhou).

Não invente integrações além de: iuclick_*, hopper_*, sonar_*, figma_*, github_*.
```

## P2. Nova skill de domínio

Requisitos: RF-A01–A03.

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
Ao final, rode `velqi validate skills/(<categoria>)/<nome>` e mostre a saída.
```

## P3. Roteamento

Requisitos: RF-B02, RF-B04.

```markdown
Crie skills/_routing.md e skills/_index.md.
_index: tabela com skill, categoria, intenção, gatilhos, nível padrão.
_routing: para cada intenção (ex.: "automatizar fluxo web a partir de card"), a sequência de skills e o nível de carga de cada uma.
Regra de desempate: se dois roteamentos casarem, pergunte ao usuário em vez de escolher.
Crie skills/_examples/routing-golden.jsonl com 30 pedidos reais e o plano esperado, para teste de regressão.
```

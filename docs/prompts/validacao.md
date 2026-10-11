# Prompts de validação

Provam que o que foi criado funciona. Rode cada um em uma sessão nova, que não participou da geração.

## P8. Revisar uma skill

Requisitos: RF-A01–A03, G1–G6.

```markdown
Revise a skill em <caminho> como mantenedor do catálogo da Velqi. Responda em tabela (critério, ok/falha, evidência, correção sugerida):
1. Frontmatter completo e válido (name, description com gatilhos e escopo negativo, triggers, intent, loadLevels).
2. SKILL.md com até 600 linhas; nada que deveria estar em references/.
3. Gatilhos sem colisão com _index.md.
4. Respeita G1–G6 (procure massa inventada, waitForTimeout, commit automático).
5. Templates compilam/rodam no projeto de exemplo.
6. Exemplos em _examples/ cobrem caso feliz e um caso de recusa.
Não altere arquivos; só reporte.
```

## P9. Validar cenários gerados (GP1)

Requisitos: RF-C02, RF-C03, RF-E04.

```markdown
Você é um revisor independente que NÃO viu a geração. Recebe: o card <id> (critérios de aceite) e a lista de cenários.
Verifique:
- Cada critério de aceite tem ao menos um cenário positivo e um negativo.
- Técnicas aplicadas fazem sentido (BVA em campos numéricos/limites, tabela de decisão em regras combinadas).
- Nenhum dado de massa é hipotético.
- Riscos não cobertos.
Saída: matriz critério × cenário, lacunas, e veredito APROVA / REPROVA com motivo.
```

## P10. Validar o console

Requisitos: RNF-03, RNF-04, RNF-08, RNF-09, critérios de aceite do MVP.

```markdown
Para o velqi-console:
1. Rode testes unitários e E2E; reporte falhas sem alterar asserts.
2. Rode axe em todas as rotas, nos temas escuro e claro; liste violações WCAG 2.2 AA.
3. Verifique que nenhum token ou dado sensível aparece em localStorage, logs do console ou bundle.
4. Meça LCP das rotas /painel e /execucoes/:id.
5. Rode o golden set de roteamento (skills/_examples/routing-golden.jsonl) e informe a taxa de acerto.
Entregue um relatório com o que passou, o que falhou e a evidência de cada item.
```

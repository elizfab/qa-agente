# Prompts

São 10 prompts prontos para colar no agente de código (Claude Code, Copilot, Cursor), divididos em três famílias.

| Família | Prompts | Para quê |
| --- | --- | --- |
| [Criação](criacao.md) | P1–P3 | Definir **quem** a Velqi é: persona, skills, roteamento |
| [Implementação](implementacao.md) | P4–P7 | Construir o **sistema em volta**: console, HITL, API, MCP |
| [Validação](validacao.md) | P8–P10 | **Provar** que funciona: revisão de skill, cenários, console |

> **Por que separar assim:** é como montar uma equipe. Primeiro você define quem a pessoa é (criação), depois dá a mesa e as ferramentas (implementação) e, por fim, faz a avaliação de desempenho (validação).

## Como usar

1. Troque tudo o que estiver entre `<>` pelo valor real.
2. Rode um prompt por vez e revise a saída antes do próximo.
3. Prompts de implementação pedem a árvore de pastas antes do código. Confirme antes de deixar o agente gerar.
4. Os prompts de validação devem rodar em uma sessão **nova**, que não viu a geração, para que o revisor não avalie o próprio trabalho.

## Como manter

- Um prompt mudou porque não funcionou bem? Registre o motivo no PR.
- Prompts citam IDs de requisitos (`RF-…`, `RNF-…`) sempre que possível, para manter a rastreabilidade.

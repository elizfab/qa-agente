# Identidade visual

Índigo é a assinatura da marca e das ações principais. O laranja `#F75F01` é a cor de destaque: marca a IA e o que pede a sua ação. Os pastéis comunicam estados. A tipografia é IBM Plex. O tema escuro é o padrão, e o claro é uma alternativa.

![Identidade visual](prototipo/identidade.png)

## Logo

| Arquivo | Uso |
| --- | --- |
| [`assets/velqi-logo.svg`](assets/velqi-logo.svg) | Logo principal, a partir de 24 px |
| [`assets/velqi-icon-16.svg`](assets/velqi-icon-16.svg) | Favicon e ícones de 16 px (sem a faísca) |

- O **V** é também um check de validação.
- A **faísca laranja** representa a inteligência da agente. Ela aparece só onde a IA atua.
- O wordmark é `velqi` em minúsculas, IBM Plex Sans 600, com letter-spacing de -0.02em a -0.03em.

## Cores principais

| Token | Hex | Uso |
| --- | --- | --- |
| `primary` | `#4B3FD1` | Botões, ações primárias, links ativos, fundo do logo |
| `primary-hover` | `#3A2FB0` (claro) / `#6256E8` (escuro) | Hover e foco |
| `indigo-soft` | `#EEEDFD` | Badges, seleções suaves, item ativo no tema claro |
| `accent` | `#F75F01` | **Destaque:** faísca da IA, contadores, item ativo do menu, "Aguardando revisão", botão "Liberar implementação" |
| `accent-text` | `#FF7A2E` (escuro) / `#B84500` (claro) | Texto em laranja: garante contraste mínimo de 4.5:1 |
| `accent-on` | `#2A1405` | Texto sobre fundo laranja (o branco não tem contraste suficiente) |

## Tema escuro (padrão)

| Token | Hex | Uso |
| --- | --- | --- |
| `background` | `#201F25` | Fundo principal |
| `surface` | `#292831` | Sidebar, cards, painéis |
| `surface-elevated` | `#32313D` | Cards destacados, menus, modais |
| `border` | `#41404C` | Bordas e divisores |
| `text-primary` | `#F5F4FA` | Texto principal |
| `text-secondary` | `#B7B5C5` | Texto auxiliar |
| `link` | `#A9A1F7` | Links sobre fundo escuro (o índigo puro não tem contraste) |

## Tema claro

| Token | Hex | Uso |
| --- | --- | --- |
| `background` | `#F7F7FB` | Fundo principal |
| `surface` | `#FFFFFF` | Cards, modais, menus |
| `surface-hover` | `#F0F0FA` | Linhas e itens selecionáveis |
| `border` | `#E4E6F0` | Cards, tabelas e divisores |
| `text-primary` | `#12131F` | Títulos e conteúdo principal |
| `text-secondary` | `#5B6072` | Descrições e metadados |

## Pastéis semânticos

Os pastéis são usados como **fundo** de badges e indicadores, sempre com texto escuro da mesma família. Eles nunca são usados como texto sobre o fundo escuro.

| Token | Fundo | Texto | Significado |
| --- | --- | --- | --- |
| `green-soft` | `#D9F3E8` | `#0B4A35` | ✓ Aprovado, concluído, sucesso |
| `yellow-soft` | `#FFF1C2` | `#5C4300` | Ⅱ Aguardando revisão humana (HITL) |
| `red-soft` | `#FADCDD` | `#7A1A22` | × Falha, bloqueio, guardrail |
| `blue-soft` | `#DCEBFC` | `#123F73` | ● Em andamento, informação |
| `purple-soft` | `#E5DFFC` | `#34237A` | IA, automações, técnicas de teste |
| `orange-soft` | `#FCE8D5` | `#6B3208` | Em análise, avisos leves |
| `pink-soft` | `#F8DDE8` | `#6E1D44` | Observações, anotações |
| `gray-soft` | `#E6E7ED` | `#33364A` | Neutro, inativo, metadados |

## Estados do agente

| Estado | Ícone | Cor | Significado |
| --- | --- | --- | --- |
| Aprovado | ✓ | `green-soft` | Execução validada e concluída |
| Aguardando | Ⅱ | `yellow-soft` | Aguardando revisão ou autorização humana |
| Bloqueado | × | `red-soft` | Uma regra impediu a continuidade do fluxo |
| Em andamento | ● | `blue-soft` | Implementação, execução de testes ou validação |

**Regra de acessibilidade:** um estado nunca é comunicado só por cor. Ele sempre vem com ícone e texto.

## Tipografia

| Fonte | Peso | Uso |
| --- | --- | --- |
| IBM Plex Sans | 600 | Títulos de páginas e seções |
| IBM Plex Sans | 500 | Subtítulos, labels e botões |
| IBM Plex Sans | 400 | Texto, conteúdo e descrições |
| IBM Plex Mono | 400–500 | IDs de execução, hashes, logs, comandos, código, metadados técnicos |

Exemplo em Mono: `EX-0142 · G4 · test:e2e · 128 cenários`

## Tokens para o Angular

Quando o console for criado, estes valores viram `src/app/design-system/tokens.css`:

```css
:root,
[data-theme="dark"] {
  --velqi-primary: #4B3FD1;
  --velqi-primary-hover: #6256E8;
  --velqi-accent: #F75F01;
  --velqi-accent-text: #FF7A2E;
  --velqi-accent-on: #2A1405;
  --velqi-bg: #201F25;
  --velqi-surface: #292831;
  --velqi-surface-elevated: #32313D;
  --velqi-border: #41404C;
  --velqi-text: #F5F4FA;
  --velqi-text-muted: #B7B5C5;
  --velqi-link: #A9A1F7;
}

[data-theme="light"] {
  --velqi-primary-hover: #3A2FB0;
  --velqi-accent-text: #B84500;
  --velqi-bg: #F7F7FB;
  --velqi-surface: #FFFFFF;
  --velqi-surface-elevated: #FFFFFF;
  --velqi-border: #E4E6F0;
  --velqi-text: #12131F;
  --velqi-text-muted: #5B6072;
  --velqi-link: #4B3FD1;
}

:root {
  --velqi-green-soft: #D9F3E8;   --velqi-green-ink: #0B4A35;
  --velqi-yellow-soft: #FFF1C2;  --velqi-yellow-ink: #5C4300;
  --velqi-red-soft: #FADCDD;     --velqi-red-ink: #7A1A22;
  --velqi-blue-soft: #DCEBFC;    --velqi-blue-ink: #123F73;
  --velqi-purple-soft: #E5DFFC;  --velqi-purple-ink: #34237A;
  --velqi-orange-soft: #FCE8D5;  --velqi-orange-ink: #6B3208;
  --velqi-pink-soft: #F8DDE8;    --velqi-pink-ink: #6E1D44;
  --velqi-gray-soft: #E6E7ED;    --velqi-gray-ink: #33364A;

  --velqi-font-sans: "IBM Plex Sans", system-ui, sans-serif;
  --velqi-font-mono: "IBM Plex Mono", ui-monospace, monospace;
}
```

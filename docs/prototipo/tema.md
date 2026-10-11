# Tema do Projeto — Identidade Visual do Console de QA

## 1. Paleta pastel — identidade visual

A ideia é manter o índigo como assinatura principal e adicionar cores pastéis para diferenciar estados, categorias e feedbacks, sem deixar o console infantil ou comprometer a legibilidade.

### 1.1. Cores principais

#### Índigo principal — Crivo

- **Cor:** `#4B3FD1`
- **Uso:** ações primárias, botões, links ativos e identidade visual.
- **Hover:** `#3A2FB0`

#### Índigo suave

- **Cor:** `#EEEDFD`
- **Uso:** fundos de badges, seleções suaves e destaques discretos.

#### Superfície clara

- **Cor:** `#F7F7FB`
- **Uso:** fundo geral no tema claro.

#### Dark base

- **Cor:** `#201F25`
- **Uso:** fundo principal do tema escuro.

### 1.2. Paleta pastel completa

Os códigos hexadecimais abaixo representam a proposta de identidade visual do projeto, não cores oficiais de uma biblioteca.

| Cor             | Token sugerido     | Hexadecimal | Aplicação                                |
| --------------- | ------------------ | ----------- | ---------------------------------------- |
| Laranja pastel  | `orange-soft`      | `#FCE8D5`   | Avisos leves e tarefas em análise        |
| Amarelo pastel  | `yellow-soft`      | `#FFF1C2`   | Aprovação pendente e atenção             |
| Tema claro      | `background-light` | `#F7F7FB`   | Fundo principal e áreas neutras          |
| Rosa pastel     | `pink-soft`        | `#F8DDE8`   | Anotações, observações e alertas suaves  |
| Azul pastel     | `blue-soft`        | `#DCEBFC`   | Informações e execução em andamento      |
| Verde pastel    | `green-soft`       | `#D9F3E8`   | Aprovado, concluído e sucesso            |
| Roxo pastel     | `purple-soft`      | `#E5DFFC`   | IA, automações e recursos inteligentes   |
| Vermelho pastel | `red-soft`         | `#FADCDD`   | Falhas, bloqueios e erros críticos       |
| Cinza pastel    | `gray-soft`        | `#E6E7ED`   | Estados neutros, bordas e itens inativos |

### 1.3. Cores de texto e bordas para o tema claro

| Token              | Hexadecimal | Uso                          |
| ------------------ | ----------- | ---------------------------- |
| `text-primary`     | `#12131F`   | Títulos e conteúdo principal |
| `text-secondary`   | `#5B6072`   | Descrições e metadados       |
| `border`           | `#E4E6F0`   | Cards, tabelas e divisores   |
| `surface-elevated` | `#FFFFFF`   | Cards, modais e menus        |
| `surface-hover`    | `#F0F0FA`   | Linhas e itens selecionáveis |

---

## 2. Tema dark — paleta completa

No modo escuro, o `#201F25` será mantido como base. As superfícies utilizarão tons ligeiramente mais claros para criar profundidade e separar visualmente os componentes.

O índigo continuará sendo a cor principal das ações, enquanto os tons pastéis serão utilizados em badges, indicadores e fundos de alertas.

### 2.1. Cores estruturais do tema dark

| Token              | Hexadecimal | Aplicação                                |
| ------------------ | ----------- | ---------------------------------------- |
| `background`       | `#201F25`   | Fundo principal do console               |
| `surface`          | `#292831`   | Sidebar, painéis e áreas secundárias     |
| `surface-elevated` | `#32313D`   | Cards destacados, menus e modais         |
| `primary`          | `#4B3FD1`   | Botões, ações e links selecionados       |
| `indigo-soft`      | `#EEEDFD`   | Destaques suaves, inclusive no tema dark |

### 2.2. Tokens semânticos do tema dark

| Token            | Hexadecimal | Aplicação              |
| ---------------- | ----------- | ---------------------- |
| `text-primary`   | `#F5F4FA`   | Texto principal        |
| `text-secondary` | `#B7B5C5`   | Texto auxiliar         |
| `border`         | `#41404C`   | Bordas e divisores     |
| `primary-hover`  | `#6256E8`   | Hover de botões        |
| `indigo-soft`    | `#EEEDFD`   | Destaques suaves       |
| `orange-soft`    | `#FCE8D5`   | Avisos                 |
| `yellow-soft`    | `#FFF1C2`   | Aprovação pendente     |
| `pink-soft`      | `#F8DDE8`   | Observações            |
| `blue-soft`      | `#DCEBFC`   | Informações e execução |
| `green-soft`     | `#D9F3E8`   | Sucesso                |
| `purple-soft`    | `#E5DFFC`   | IA e automações        |
| `red-soft`       | `#FADCDD`   | Falhas e bloqueios     |
| `gray-soft`      | `#E6E7ED`   | Estado neutro          |

### 2.3. Regras de contraste e legibilidade

Os tons pastéis são indicados principalmente para fundos de badges e indicadores, mas não devem ser usados automaticamente como texto sobre o fundo escuro.

- Utilizar textos escuros sobre badges com fundos pastéis.
- Utilizar textos claros sobre superfícies escuras.
- Validar o contraste de botões, links, textos, bordas e demais elementos interativos.
- Manter os estados identificáveis tanto pela cor quanto pelo texto ou ícone correspondente.

### 2.4. Estados do agente

#### Aprovado

- **Indicador:** `✓`
- **Categoria:** sucesso.
- **Significado:** execução validada e concluída.
- **Cor de referência:** verde pastel `#D9F3E8`.

#### Aguardando

- **Indicador:** `Ⅱ`
- **Categoria:** _Human-in-the-loop_.
- **Significado:** aguardando revisão ou autorização humana.
- **Cor de referência:** amarelo pastel `#FFF1C2`.

#### Bloqueado

- **Indicador:** `×`
- **Categoria:** falha ou _guardrail_.
- **Significado:** uma regra impediu a continuidade do fluxo.
- **Cor de referência:** vermelho pastel `#FADCDD`.

#### Em andamento

- **Indicador:** `●`
- **Categoria:** execução ativa.
- **Significado:** implementação, execução de testes ou validação.
- **Cor de referência:** azul pastel `#DCEBFC`.

---

## 3. Tipografia — IBM Plex Sans

A **IBM Plex Sans** será a fonte oficial do produto, tanto no tema claro quanto no escuro. Ela oferece uma identidade técnica e profissional sem deixar a interface visualmente pesada.

### 3.1. Fonte principal

**IBM Plex Sans**

- **Aplicação:** tipografia padrão da interface do console de QA.
- **Peso 400 — Regular:** textos, conteúdo e descrições.
- **Peso 500 — Medium:** subtítulos, rótulos (_labels_) e botões.
- **Peso 600 — SemiBold:** títulos de páginas e seções.

### 3.2. Hierarquia tipográfica

#### Títulos

- Exemplo: `Painel de qualidade`
- Peso: `600 — SemiBold`
- Aplicação: títulos de páginas e seções.

#### Subtítulos e elementos de interface

- Exemplo: `Execuções aguardando aprovação`
- Peso: `500 — Medium`
- Aplicação: subtítulos, labels e botões.

#### Texto principal

- Exemplo: `Resultado da análise, cenários testados e evidências encontradas.`
- Peso: `400 — Regular`
- Aplicação: conteúdo, descrições e informações da execução.

### 3.3. Fonte complementar — IBM Plex Mono

A **IBM Plex Mono** será utilizada opcionalmente para informações técnicas que precisam de alinhamento visual e fácil leitura.

Exemplo:

```text
EX-0142 · G4 · test:e2e · 128 cenários
```

Aplicações recomendadas:

- IDs de execução.
- Hashes e identificadores.
- Logs.
- Comandos.
- Trechos de código.
- Metadados técnicos.

### 3.4. Diretriz tipográfica

Utilizar a IBM Plex Sans em toda a interface e a IBM Plex Mono somente para informações técnicas. Essa combinação cria uma hierarquia visual clara, preservando a consistência e a legibilidade do console.

---

## 4. Diretrizes gerais da identidade visual

1. **Identidade principal:** índigo `#4B3FD1`.
2. **Tema claro:** fundo `#F7F7FB`, superfícies brancas e textos escuros.
3. **Tema escuro:** fundo `#201F25`, superfícies em tons mais claros e textos claros.
4. **Cores semânticas:** utilizar a paleta pastel para comunicar estados, categorias e feedbacks.
5. **Legibilidade:** validar o contraste entre texto, fundo, bordas e elementos interativos.
6. **Estados acessíveis:** nunca depender exclusivamente da cor para comunicar sucesso, falha, aprovação ou bloqueio.
7. **Tipografia:** IBM Plex Sans como fonte principal e IBM Plex Mono como complemento técnico.
8. **Consistência:** manter os mesmos significados semânticos para as cores nos temas claro e escuro, adaptando os contrastes quando necessário.

## 5. Objetivo visual do projeto

Criar uma identidade visual para um console de QA moderno, profissional e intuitivo, que utilize o índigo como assinatura principal, cores pastéis para feedbacks semânticos e suporte consistente aos temas claro e escuro.

A interface deve transmitir qualidade, confiança, inteligência, rastreabilidade e clareza, mantendo uma experiência visual adequada a uma plataforma de automação de testes e análise de qualidade de software.

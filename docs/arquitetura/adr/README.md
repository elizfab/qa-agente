# Architecture Decision Records (ADRs)

Uma ADR registra **uma** decisão de arquitetura: o contexto, a decisão e as consequências.

> **Analogia:** é a ata de reunião das decisões técnicas. Daqui a seis meses, quando alguém perguntar "por que Go e não NestJS?", a resposta está aqui com a data e o motivo, e não na memória de quem estava na sala.

## Regras

1. Copie o [`0000-template.md`](0000-template.md) com o próximo número: `NNNN-titulo-em-kebab-case.md`.
2. Uma ADR começa como **Proposta** e vira **Aceita** quando aprovada em PR.
3. Uma ADR aceita não se edita. Se a decisão mudar, crie uma nova ADR e marque a antiga como **Substituída por NNNN**.

## Índice

| # | Decisão | Status |
| --- | --- | --- |
| [0001](0001-registrar-decisoes-com-adr.md) | Registrar decisões de arquitetura com ADRs | Aceita |
| [0002](0002-nome-velqi.md) | Nome do produto: Velqi | Aceita (pendente de domínio e INPI) |
| [0003](0003-console-em-angular.md) | Console web em Angular | Aceita |
| [0004](0004-linguagem-do-bff.md) | Linguagem da API (BFF): Go ou NestJS | Proposta |
| [0005](0005-onde-o-agente-roda.md) | Onde o agente roda: máquina do dev ou runner central | Proposta |

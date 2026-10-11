# 0005. Onde o agente roda: máquina do dev ou runner central

- **Status:** Proposta
- **Data:** 2026-10-10
- **Decisores:** a definir

## Contexto

No sistema de referência, o agente roda na máquina do dev, dentro do editor. O console da Velqi precisa ver e controlar as execuções (pausar no HITL, liberar, auditar).

## Opções consideradas

| Opção | Prós | Contras |
| --- | --- | --- |
| Máquina do dev, reportando eventos à API | Usa as credenciais e o ambiente que o dev já tem; menos infraestrutura | O console não dispara execuções; depende da máquina ligada; auditoria menos confiável |
| Runner central (headless), disparado pela API | O console controla o ciclo inteiro; execução reproduzível; auditoria forte | Exige infraestrutura, gestão de segredos e acesso de rede aos sistemas via MCP |
| Híbrido: começa local, migra para runner | MVP mais rápido; o caminho para o runner fica aberto | Dois modos para manter durante a transição |

## Decisão

A decidir.

## Consequências

Serão preenchidas quando a decisão for tomada.

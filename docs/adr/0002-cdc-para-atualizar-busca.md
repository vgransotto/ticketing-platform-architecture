# ADR 0002 — Usar CDC com Debezium e Kafka para alimentar a busca

- **Status:** aceita

## Contexto

O Elasticsearch precisa refletir as criações e alterações de eventos feitas no banco, sem que o event-service precise publicar nos dois lugares. Escrever no banco e no índice em duas operações separadas pode deixar os dois fora de sincronia se uma delas falhar.

## Decisão

Usar **Debezium** para capturar as alterações do banco (CDC), publicá-las no **Kafka**, e um **Worker** consumir as mensagens e atualizar o **Elasticsearch**.

## Consequências

- **Positivo:** a atualização do índice acontece a partir do log do banco, sem código duplicado nos serviços.
- **Positivo:** o Kafka permite reprocessar as mensagens se o índice precisar ser reconstruído.

## Alternativas consideradas

- **Escrita dupla pelo serviço** (banco e Elasticsearch na mesma operação). Simples, mas sujeita a inconsistência se uma das escritas falhar.

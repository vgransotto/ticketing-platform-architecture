# Arquitetura

## Visão geral

O tráfego entra pelo **Load Balancer** e pelo **API Gateway**, que o encaminham aos serviços. Cada serviço cuida de uma responsabilidade:

| Componente | Responsabilidade |
|---|---|
| **API Gateway** | Ponto de entrada das requisições, encaminhando-as aos serviços. |
| **event-service** | Recebe a criação de eventos e devolve os seus detalhes. |
| **search-service** | Realiza buscas inteligentes com Fuzzy Search, usando o Elasticsearch como motor. |
| **booking-service** | Processa as reservas do usuário. |
| **Worker** | Consome as alterações publicadas no Kafka e atualiza o Elasticsearch. |
| **Redis** | Cache de detalhes de evento e locks temporários de reserva. |
| **Kafka** | Transporta as alterações de dados do banco para o worker. |
| **Debezium** | Captura as criações e alterações de registros no banco (CDC). |
| **Elasticsearch** | Índice de busca de eventos. |

## Fluxos principais

**Pesquisa de eventos**
1. O cliente chega pelo API Gateway.
2. O search-service consulta o Elasticsearch com Fuzzy Search.
3. Os detalhes do evento vêm do cache Redis e, em caso de miss, do banco.

**Criação ou alteração de evento**
1. O event-service grava no banco.
2. O Debezium captura a alteração (CDC) e publica no Kafka.
3. O Worker consome a mensagem e atualiza o Elasticsearch.

**Reserva**
1. O booking-service trava o assento no Redis com prazo de expiração.
2. A confirmação da reserva é gravada no banco. Ver [Fluxo de reserva](06-fluxo-de-reserva.md) e a [ADR 0003](adr/0003-redis-para-cache-e-locks.md).

## Diagramas

Os diagramas estão em [diagrams/](../diagrams/).

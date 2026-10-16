# Escalabilidade

Este documento registra os problemas de escala identificados na arquitetura e como cada um foi tratado.

## 1. Ponto único de falha

**Problema:** se um serviço ou instância cair, a aplicação para.

**Tratamento:** escalar horizontalmente. Se uma instância cair, outra sobe imediatamente, e o load balancer passa a direcionar as requisições às instâncias ativas. Ver [ADR 0001](adr/0001-separar-servicos.md).

## 2. Sobrecarga no banco de dados

**Problema:** o banco recebe consultas repetidas por dados que raramente mudam, como os detalhes do evento.

**Tratamento:** uma camada de cache em memória (Redis) absorve a maioria dessas consultas. Ver [ADR 0003](adr/0003-redis-para-cache-e-locks.md).

## 3. Escalabilidade do banco de dados

**Problema:** o banco é o gargalo de escrita e de leitura no pico.

**Tratamento:** banco de escrita com réplicas de leitura. O cache reduz as consultas diretas ao banco, e o CDC alimenta a busca sem sobrecarregar as leituras. Ver [Estimativas](02-estimativas.md).

## 4. Escalabilidade dos serviços

**Problema:** um único serviço concentra todo o tráfego de pesquisa, leitura e reserva.

**Tratamento:** separar os serviços por responsabilidade ([ADR 0001](adr/0001-separar-servicos.md)) e escalar cada um de forma independente.

## 5. Performance em alta volumetria

**Problema:** a pesquisa precisa responder em menos de 500 ms no pico.

**Tratamento:** cache para dados de leitura frequente e Elasticsearch para a busca, alimentado por CDC ([ADR 0002](adr/0002-cdc-para-atualizar-busca.md)).
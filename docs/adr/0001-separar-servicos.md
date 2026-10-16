# ADR 0001 — Separar o sistema em serviços por responsabilidade

- **Status:** aceita

## Contexto

Pesquisa, leitura de eventos e reserva têm perfis de carga muito diferentes. A pesquisa e a leitura são massivas e tolerantes a dados levemente desatualizados. A reserva é pouca em volume, mas exige consistência forte. Num único serviço, o pico de uma função compete por recursos com as outras.

## Decisão

Separar o sistema em `event-service`, `search-service` e `booking-service`, cada um com uma responsabilidade, e escalar cada um de forma independente atrás de um API Gateway e um Load Balancer.

## Consequências

- **Positivo:** o pico de pesquisa não derruba a reserva. Cada serviço escala conforme a sua demanda.
- **Positivo:** eliminar o único ponto de falha passa a ser uma questão de número de instâncias, não de redesenho.
- **Negativo:** os dados ficam em mais de um lugar, o que exige sincronização (ver [ADR 0002](0002-cdc-para-atualizar-busca.md)).

## Alternativas consideradas

- **Monólito.** Mais simples no início, mas acopla o pico de leitura à reserva.

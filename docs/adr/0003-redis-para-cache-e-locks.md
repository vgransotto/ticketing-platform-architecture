# ADR 0003 — Usar Redis para cache e locks temporários de reserva

- **Status:** aceita

## Contexto

Os detalhes do evento mudam raramente, mas são lidos o tempo todo. Consultar o banco a cada leitura não sustenta o pico. Além disso, a reserva precisa travar um assento por tempo limitado, enquanto o usuário conclui a compra.

## Decisão

Usar **Redis Sentinel HA** para:

1. **Cache** dos detalhes do evento, protegendo o banco da maioria das consultas.
2. **Lock de reserva temporária**, com prazo de expiração definido, liberando o assento se a compra não for concluída.

## Consequências

- **Positivo:** leitura em memória, com latência muito baixa para a pesquisa.
- **Positivo:** o prazo de expiração libera assentos presos sem intervenção manual.

## Mitigação

- Usar o banco como fonte da verdade do assento, com lock de linha ou controle otimista, de modo que a venda só se confirme se o assento ainda estiver disponível.
- Tratar o Redis como camada de performance e de reserva temporária, não como garantia final.

## Alternativas consideradas

- **Lock apenas no banco.** Garante consistência, mas aumenta a carga de escrita no pico.
- **Redis Cluster com consenso** (ex.: Redlock). Reduz o risco, mas adiciona complexidade operacional e não elimina todos os casos de falha.

# ADR 0004 — Usar Elasticsearch como motor de busca

- **Status:** aceita

## Contexto

A pesquisa precisa ser tolerante a erros de digitação e encontrar palavras por proximidade, com latência abaixo de 500 ms no pico. Consultas de texto livre com essas características não são eficientes em um banco relacional tradicional.

## Decisão

Usar **Elasticsearch** como motor de busca, com Fuzzy Search, alimentado pelo fluxo de CDC descrito na [ADR 0002](0002-cdc-para-atualizar-busca.md).

## Consequências

- **Positivo:** busca textual com tolerância a erros e relevância, em escala horizontal.
- **Positivo:** o banco continua como fonte da verdade, e o índice é derivado dele.

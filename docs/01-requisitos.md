# Requisitos

## Funcionais

- **Visualização de eventos:** os usuários devem poder visualizar os eventos disponíveis na plataforma.
- **Pesquisa de eventos:** os usuários devem poder pesquisar eventos específicos.
- **Compra de ingressos:** os usuários devem poder comprar ingressos para participar dos eventos.

## Não funcionais

| Requisito | Descrição |
|---|---|
| Escala de pico | Suportar 10 milhões de usuários ativos simultaneamente em dias de grandes eventos. |
| Pesquisa inteligente | Tolerar erros de digitação e encontrar palavras por proximidade (stemming). Ex.: "casa" retorna "casas", "casarão", "casinha". |
| Consistência | Impedir que dois usuários comprem o mesmo ingresso ou assento. |
| Baixa latência | Pesquisa de eventos em menos de 500 ms. |
| Alta taxa de leitura | Suportar leitura na proporção 100:1 em relação à escrita. |
| Alta disponibilidade | Operar em modo de alta disponibilidade, sem ponto único de falha. |

# Relatório de insights — Festa em Cena

## Qualidade dos dados

A base tem 1.500 pedidos de agosto a novembro de 2024 e 2025. Antes de comparar canais e períodos, foi preciso corrigir e marcar problemas: 18 categorias vazias foram preenchidas com "sem categoria" e 131 foram padronizadas (ex.: "  FANTASIA!!!" virou "Fantasia"); 45 canais tiveram a escrita corrigida. Também foram marcados 7 pedidos com quantidade -1, 12 com preço vazio e 4 com data de venda inválida. Nenhum valor foi inventado: os registros problemáticos continuam na base, identificados por colunas de marcação (flags), como problema_quantidade, problema_preco e data_invalida.

## Insight 1 — entrega sem data não significa cancelamento

A hipótese de que todo pedido sem data de entrega foi cancelado é falsa. Dos 143 pedidos sem entrega, 109 estão cancelados, mas 34 aparecem como Concluído. Todo cancelado está sem entrega, mas o contrário não vale. Esses 34 pedidos podem ser falhas de registro ou retiradas na loja; a base não informa o motivo. Sugere-se revisar o processo de registro de entregas.

## Insight 2 — a Web é o principal canal de pedidos

A Web registrou 666 pedidos (44,4%), a Loja física 551 (36,7%) e o Mercado 283 (18,9%). O site deve ser prioridade em estoque e experiência de compra na temporada. Limitação: são pedidos, não unidades nem receita; o Mercado pode ter mais unidades por pedido.

## Insight 3 — outubro concentra mais da metade dos pedidos

Outubro teve 393 pedidos em 2024 (52,5% do ano) e 410 em 2025 (54,8%). Novembro, após o Halloween, ainda superou agosto e setembro (174 e 182, contra no máximo 104). Promoções pós-Halloween são uma possível explicação; um próximo passo seria analisar a coluna de descontos. Como o negócio é sazonal, vale estudar outras datas temáticas, mas esta base não tem dados sobre elas.

## Limitações

As contagens incluem pedidos cancelados e representam pedidos registrados, não vendas concluídas. A base é fictícia e cobre apenas agosto a novembro.

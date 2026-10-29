# Festa em Cena — análise de pedidos de Halloween

## Descrição do projeto

A Festa em Cena é uma rede fictícia que vende fantasias e acessórios de
Halloween pelos canais Mercado, Loja física e Web. [Explicar o problema
com suas palavras.]

### O que uma linha representa

Cada linha representa um pedido de um único produto. A coluna `quantidade`
informa quantas unidades desse produto foram pedidas: uma linha com
quantidade 3 conta como 1 pedido e 3 unidades.

### Colunas principais

- `categoria`: [a preencher]
- `canal_venda`: [a preencher]
- `data_venda`: [a preencher]
- `data_entrega`: [a preencher]

### Regras de limpeza adotadas

**Textos (categoria e canal)**
- Categoria vazia é preenchida com "sem categoria", sem adivinhar o valor real.
- Categoria: espaços das pontas removidos (`strip`), letras minúsculas (`lower`) e
  pontuação removida com `re.sub`, mantendo apenas letras e espaços. Como as
  categorias deste CSV não usam acentos, o padrão aceita apenas `a-z`. Se nada
  sobrar após a limpeza, o valor vira "sem categoria".
- Canal: padronizado para Mercado, Loja física e Web com um dicionário.
  Um canal desconhecido fica vazio para ser investigado, sem ser inventado.
- 18 categorias vazias foram preenchidas e 131 categorias não vazias foram
  corrigidas; 45 canais foram padronizados.

**Valores numéricos (quantidade e preço)**
- `quantidade` é válida quando é um número inteiro maior que zero.
- `preco_unitario` é válido quando está preenchido e não é negativo (zero é aceito).
- Registros inválidos não são apagados nem corrigidos: recebem uma marcação
  (flag) para ficarem fora das análises que dependem desses valores.
- Preço vazio não é preenchido com zero, porque vazio significa "desconhecido".
- Resultado: 7 quantidades inválidas (`-1`), 12 preços vazios, 0 pedidos com
  os dois problemas, 19 pedidos afetados no total.
- `custo_unitario` e `desconto_pct` não foram validados, pois não são usados
  nesta análise (uso opcional segundo o enunciado).
  
## Guia de execução

**Datas**
- `data_venda` foi convertida com `datetime.strptime()` no formato `%Y-%m-%d %H:%M:%S`.
  O texto original foi preservado em `data_venda_original`.
- Foram extraídos ano, mês, dia e hora, e criada a apresentação brasileira
  (`dd/mm/aaaa`) com `strftime()`.
- 4 datas inválidas (`data_invalida`) foram marcadas e ficaram fora da contagem
  por ano-mês. Nenhuma foi substituída pela data de hoje.
- As contagens por canal e por ano-mês incluem todos os status (são pedidos
  registrados, não vendas concluídas). Ano-mês é usado em vez de só mês,
  para não misturar 2024 e 2025.

## Resultados

## Resultados

### Resumo da qualidade dos dados

- 1.500 pedidos lidos (14 colunas).
- Categoria: 18 vazias preenchidas com "sem categoria" e 131 corrigidas.
- Canal: 45 padronizados; nenhum desconhecido.
- 7 quantidades inválidas (`-1`) e 12 preços vazios; 19 pedidos afetados.
- 4 datas de venda inválidas.

### Hipótese da data de entrega

A hipótese de que todo pedido sem data de entrega foi cancelado é **falsa**.
Dos 143 pedidos sem data de entrega, 109 estão cancelados, mas 34 têm status
Concluído. Esses 34 são contraexemplos suficientes para derrubar a hipótese.
Por outro lado, todos os 109 pedidos cancelados estão sem data de entrega, ou
seja, a relação vale em um sentido ("todo cancelado não tem entrega"), mas não
no outro. **Interpretação:** um pedido Concluído sem data de entrega pode ser
uma falha de registro ou uma retirada na loja. **Limitação:** o dado não
informa qual dessas explicações é a correta, nem se há outra. Por isso, o
status e as datas não foram alterados, apenas marcados na coluna `sem_entrega`.

### Descoberta 1 — canal

A Web concentra o maior número de pedidos registrados: 666 (44,4%), seguida
pela Loja física, com 551 (36,7%), e pelo Mercado, com 283 (18,9%). Nenhum
pedido ficou de fora por canal desconhecido. **Interpretação:** o site é o
principal canal de entrada de pedidos da Festa em Cena, o que sugere priorizar
a experiência de compra e o estoque disponível para a Web na temporada.
**Limitação:** a contagem é de pedidos, e não de unidades nem de receita. Como
um pedido pode ter várias unidades, o Mercado pode ter menos pedidos e ainda
assim vender muitas fantasias por pedido. Além disso, a contagem inclui pedidos
cancelados e a base é fictícia.

### Descoberta 2 — período

Outubro concentra a maior parte dos pedidos registrados nos dois anos: 393 em
2024 (52,5% do ano) e 410 em 2025 (54,8%). Novembro, depois do Halloween, ainda
teve 174 e 182 pedidos, mais que agosto e setembro (no máximo 104). Uma possível
explicação são promoções pós-Halloween ou festas realizadas depois da data, mas
os dados não informam o motivo. **Limitação:** as contagens incluem pedidos
cancelados, cobrem apenas agosto a novembro e a base é fictícia. 4 pedidos com
data inválida ficaram fora desta contagem.
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

## Guia de execução

[Sprint 5]

## Resultados

[Sprint 5]
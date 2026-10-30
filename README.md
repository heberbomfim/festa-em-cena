# Festa em Cena — análise de pedidos de Halloween

Mini-projeto avaliativo do Módulo 1 — Machine Learning e Visão Computacional (SCTEC).

## Descrição do projeto

A **Festa em Cena** é uma rede fictícia que vende fantasias e acessórios de
Halloween por três canais: **Mercado** (caixas de supermercados parceiros),
**Loja física** (lojas próprias) e **Web** (site). A empresa quer comparar
como os pedidos variam por canal e por período, mas os registros têm
problemas: categorias vazias ou escritas de formas diferentes, canais com
grafias inconsistentes, quantidades inválidas, preços ausentes e datas
inválidas.

O objetivo deste programa é **ler, organizar e explorar** o arquivo
`halloween_vendas.csv` com Python e Pandas, identificando e marcando esses
problemas **sem inventar valores**, e produzir um relatório simples antes de
qualquer comparação ou uso em Machine Learning.

### O que uma linha representa

Cada linha representa **um pedido de um único produto**. A coluna `quantidade`
informa quantas unidades desse produto foram pedidas: uma linha com
quantidade 3 conta como **1 pedido** e **3 unidades**. Por isso, contar
pedidos é contar linhas, e não somar quantidades.

A base tem 1.500 pedidos, de agosto a novembro de 2024 e de 2025. Os dados
são fictícios e não representam o mercado real.

### Colunas principais

- `categoria`: tipo de produto (Fantasia ou Acessorio). Tinha valores vazios,
  espaços extras, maiúsculas e pontuação indevida.
- `canal_venda`: canal em que o pedido foi registrado (Mercado, Loja física
  ou Web). Tinha variações de escrita, como " web".
- `data_venda`: data e hora do registro do pedido, no formato
  `aaaa-mm-dd hh:mm:ss`, horário local de São Paulo.
- `data_entrega`: data de entrega do pedido. Está vazia em 143 pedidos.

### Regras de limpeza adotadas

**Leitura**
- O CSV é lido com `pd.read_csv()`, separador vírgula, `encoding="utf-8"` e
  todas as colunas como texto (`dtype=str`), para que valores problemáticos
  apareçam como vieram e sejam convertidos depois com regras próprias.
- O DataFrame original (`df_original`) nunca é alterado; o trabalho é feito
  em uma cópia (`df_org`). O arquivo CSV também não é modificado.

**Textos (categoria e canal)**
- Categoria vazia é preenchida com "sem categoria", sem adivinhar o valor real.
- Categoria: espaços das pontas removidos (`strip`), letras minúsculas (`lower`)
  e pontuação removida com `re.sub(r"[^a-z\s]", "", texto)`, mantendo apenas
  letras e espaços. Como as categorias deste CSV não usam acentos, o padrão
  aceita apenas `a-z`. Se nada sobrar após a limpeza, o valor vira
  "sem categoria". A primeira letra volta a ser maiúscula só para apresentação.
- Canal: padronizado para Mercado, Loja física e Web com um dicionário.
  Um canal desconhecido fica vazio para ser investigado, sem ser inventado.
- Os textos originais ficam em `categoria_original` e `canal_original`.

**Valores numéricos (quantidade e preço)**
- `quantidade` é válida quando é um número inteiro maior que zero.
- `preco_unitario` é válido quando está preenchido e não é negativo (zero é aceito).
- Registros inválidos não são apagados nem corrigidos: recebem uma marcação
  (flag) para ficarem fora das análises que dependem desses valores.
- Preço vazio não é preenchido com zero, porque vazio significa "desconhecido".
- A conversão usa `try/except`, para que um valor impossível de converter não
  interrompa o programa.
- `custo_unitario` e `desconto_pct` não foram validados, pois não são usados
  nesta análise (uso opcional segundo o enunciado).

**Datas**
- `data_venda` foi convertida com `datetime.strptime()` no formato
  `%Y-%m-%d %H:%M:%S`. O texto original foi preservado em `data_venda_original`.
- Foram extraídos ano, mês, dia e hora, e criada a apresentação brasileira
  (`dd/mm/aaaa`) com `strftime()`.
- Datas inválidas foram marcadas na coluna `data_invalida` e ficaram fora da
  contagem por ano-mês. Nenhuma foi substituída pela data de hoje.
- As contagens por canal e por ano-mês incluem todos os status (são pedidos
  registrados, não vendas concluídas). Ano-mês é usado em vez de só mês,
  para não misturar 2024 e 2025.

## Guia de execução

### Arquivos necessários

Todos devem estar **na mesma pasta**:

- `analise.ipynb`: notebook com o código da análise.
- `halloween_vendas.csv`: base de dados original.
- `README.md`: esta documentação.
- `report.md`: relatório de insights.

### Requisitos

- Python 3 (desenvolvido com Python 3.14).
- Bibliotecas `pandas` e `ipykernel`: `pip install pandas ipykernel`.
- VS Code com as extensões **Python** e **Jupyter**, ou Jupyter Notebook.
  Os módulos `re`, `datetime` e `pathlib` já vêm com o Python.

### Como executar

1. Baixe ou clone o repositório:
   `git clone https://github.com/heberbomfim/festa-em-cena.git`
2. Abra a pasta `festa-em-cena` no VS Code.
3. Abra `analise.ipynb` e selecione o kernel Python 3.
4. Clique em **Run All**. As células devem ser executadas em ordem, de cima
   para baixo. Se o arquivo não for encontrado, confira se o CSV está na mesma
   pasta do notebook e se o nome está correto.

### Organização do código

| Função | Responsabilidade |
|---|---|
| `ler_csv(caminho)` | lê o CSV como texto e devolve o DataFrame |
| `limpar_categoria(valor)` | aplica `strip`, `lower` e `re.sub`; vazio vira "sem categoria" |
| `padronizar_canal(valor)` | traduz o canal para o nome oficial com um dicionário |
| `eh_numero(texto)` | diz se um texto pode ser convertido em número |
| `converter_quantidade(texto)` | devolve a quantidade inteira válida, ou vazio |
| `converter_preco(texto)` | devolve o preço válido, ou vazio |
| `converter_data(texto)` | converte `data_venda` com `strptime`, ou devolve vazio |
| `formatar_data_br(data)` | apresenta a data como `dd/mm/aaaa` com `strftime` |
| `data_entrega_vazia(valor)` | diz se a data de entrega está vazia |
| `gerar_relatorio_status(df)` | reúne os números de qualidade em um dicionário |

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

## Reflexão: Machine Learning e qualidade de dados

Um modelo de Machine Learning aprende padrões a partir dos dados que recebe.
Se os dados têm erros, o modelo aprende os erros ("garbage in, garbage out").
Neste projeto, sem a padronização, "Fantasia", " Fantasia" e "FANTASIA!!!"
seriam tratadas como três categorias diferentes, e um modelo veria três grupos
onde existe apenas um. Da mesma forma, preencher um preço vazio com zero
ensinaria ao modelo que existem produtos gratuitos, e trocar uma data inválida
pela data de hoje criaria pedidos em um período em que eles não aconteceram.

Por isso, a limpeza deste projeto segue dois princípios: **padronizar o que é
claramente o mesmo valor** e **marcar, sem inventar, o que é desconhecido**.
As marcações (flags) permitem decidir depois, conforme o objetivo, se um
registro deve ser excluído ou tratado de outra forma.

Mas dados limpos não bastam. Um modelo pode sofrer **overfitting** (decorar
os dados de treino e falhar em dados novos) ou **underfitting** (ser simples
demais e não captar o padrão). Evitá-los depende também da
**representatividade** dos dados, da escolha do modelo e da avaliação com
dados que o modelo não viu. Esta base, por exemplo, cobre apenas agosto a
novembro: um modelo treinado nela não deveria ser usado para prever pedidos
de dezembro ou de outras datas comemorativas, porque esses períodos não estão
representados.
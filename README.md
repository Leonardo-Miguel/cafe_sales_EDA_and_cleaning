# dirty_cafe_sales — Análise e limpeza de dados de vendas de cafeteria

Projeto de estudo prático de engenharia e ciência de dados no Databricks: a partir de um dataset "sujo" de vendas de uma cafeteria (10.000 transações), faz-se a exploração, a limpeza dos dados e a persistência de uma tabela tratada para análises futuras.

## Dataset utilizado

Os dados utilizados neste projeto estão disponíveis no Kaggle:

🔗 [Cafe Sales — Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)

O dataset contém 10.000 transações de uma cafeteria com diversos problemas de qualidade (valores inválidos, inconsistentes e faltantes), ideal para praticar limpeza e tratamento de dados.

## Estrutura do projeto

| Notebook | Descrição |
| --- | --- |
| `0_csv_to_table` | Carrega o CSV bruto (10.000 transações) do dataset `cafe_sales` e o grava como tabela Delta no catálogo do Databricks, servindo como base para as análises subsequentes. |
| `1_exploratory_analysis` | Análise exploratória dos dados brutos: inspeciona o esquema, verifica distribuições das colunas, identifica valores inválidos ou inconsistentes (ex.: `UNKNOWN`, `ERROR`) e quantifica dados faltantes. |
| `2_data_cleaning` | Limpeza e padronização dos dados: substitui valores inválidos por nulos, converte tipos adequados, preenche dados faltantes quando possível e persiste a tabela tratada para consumo analítico. |

## Conclusões

A tabela abaixo compara a quantidade de valores inválidos (nulos) por coluna **antes** do tratamento (após a padronização de `ERROR`, `UNKNOWN` e `NULL` para nulo) e **depois** de todo o processo de limpeza (`show_invalids` do notebook `2_data_cleaning`):

| Coluna | Inválidos antes | Inválidos depois | Redução |
| --- | --- | --- | --- |
| `item` | 969 | 6 | 99,4% |
| `quantity` | 479 | 23 | 95,2% |
| `price_per_unit` | 533 | 6 | 98,9% |
| `total_spent` | 502 | 23 | 95,4% |
| `payment_method` | 3178 | 3178 | 0% |
| `location` | 3961 | 3961 | 0% |
| `transaction_date` | 460 | 460 | 0% |

* `transaction_id` e `numeric_id` não possuíam valores inválidos.
* As colunas numéricas e de item foram quase integralmente preenchidas: restam apenas 6 inválidos em `item` e `price_per_unit` e 23 em `quantity` e `total_spent` (linhas em que nenhuma estratégia de cálculo era aplicável).
* `payment_method`, `location` e `transaction_date` permanecem sem preenchimento: as análises mostraram que não há relação entre elas, nem com os produtos, e que o ID da transação não cresce com o tempo — logo não há base para inferência. A recomendação é mantê-las como desconhecidas e, se necessário, explorar algoritmos de Machine Learning para um preenchimento mais profundo.



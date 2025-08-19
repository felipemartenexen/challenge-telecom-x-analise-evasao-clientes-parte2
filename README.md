# Análise de Evasão de Clientes (Churn) em Telecomunicações

## Propósito da Análise

O objetivo principal deste projeto é realizar uma análise aprofundada dos dados de clientes de uma empresa de telecomunicações para **prever a evasão de clientes (Churn)**. Através da identificação dos fatores que mais contribuem para que os clientes cancelem seus serviços, buscamos fornecer insights acionáveis para subsidiar a tomada de decisões estratégicas e desenvolver estratégias de retenção mais eficazes.

## Estrutura do Projeto

A estrutura deste projeto está organizada da seguinte forma:

-   `nome_do_seu_notebook.ipynb`: O notebook principal contendo todo o código e análise realizada (desde o carregamento dos dados até a modelagem preditiva).
-   `TelecomX_Data.json`: O arquivo original com os dados dos clientes.
-   [Opcional] `data/`: Pasta para armazenar dados processados (por exemplo, um arquivo CSV com os dados limpos e tratados, se você optar por salvá-lo).
-   [Opcional] `visualizations/`: Pasta para armazenar visualizações geradas durante a Análise Exploratória de Dados (EDA).

## Processo de Preparação dos Dados

A preparação dos dados foi uma etapa crucial para garantir a qualidade e o formato adequado para a análise e modelagem. As principais etapas realizadas no notebook incluíram:

1.  **Carregamento e Achatamento:** Os dados foram carregados a partir do arquivo JSON e as estruturas aninhadas (`customer`, `phone`, `internet`, `account`) foram achatadas para criar um DataFrame tabular (`dados_flattened`).
2.  **Classificação e Tratamento de Variáveis:** As variáveis foram exploradas e classificadas em categóricas e numéricas. Inconsistências como a importação de `Charges.Total` como objeto e valores vazios em `Churn` foram identificadas.
3.  **Limpeza de Dados:** Linhas com valores faltantes em `Charges.Total` (após a conversão para numérico) e linhas com valores vazios em `Churn` foram removidas (`dados_cleaned`).
4.  **Engenharia de Features:** A coluna `Contas_Diarias` foi criada dividindo `Charges.Monthly` por 30.
5.  **Codificação de Variáveis Categóricas:** As variáveis categóricas (exceto a original `Churn`) foram transformadas em formato numérico utilizando **One-Hot Encoding** (`dados_encoded`). A coluna `Churn` foi convertida para `Churn_numeric` (0 para 'No', 1 para 'Yes') para a modelagem.
6.  **Tratamento de Desequilíbrio de Classes:** Dada a proporção desigual entre as classes 'Churn' (verificada na célula com id `005bb312`), a técnica **SMOTE (Synthetic Minority Over-sampling Technique)** foi aplicada para balancear o conjunto de dados (`X_resampled`, `y_resampled`) (implementado na célula com id `963c3b00`).

## Modelagem Preditiva

Para prever o Churn, foram treinados dois modelos de classificação:

-   **Regressão Logística:** Um modelo linear que modela a probabilidade de Churn. **Justificativa:** Simples, interpretável e um bom ponto de partida. **Necessidade de Normalização:** Sim, os dados de treino e teste foram **padronizados** usando `StandardScaler` (`X_train_scaled`, `X_test_scaled`) antes de treinar este modelo, pois modelos lineares são sensíveis à escala das features (conforme explicado nas células com ids `f5e6e456` e `4176924f`).
-   **Random Forest:** Um modelo de ensemble baseado em árvores de decisão. **Justificativa:** Robusto, geralmente apresenta bom desempenho e pode capturar interações não lineares. **Necessidade de Normalização:** Não, este modelo foi treinado nos dados balanceados **não escalados** (`X_train`, `X_test`), pois modelos baseados em árvores não são sensíveis à escala das features (conforme explicado nas células com ids `f5e6e456` e `ae9746df`).

## Análise Exploratória de Dados (EDA) e Insights

A EDA revelou insights importantes sobre os fatores associados ao Churn. Alguns exemplos de análises e visualizações incluídas no notebook são:

-   Distribuição da variável alvo `Churn` (gráfico na célula com id `45274794`).
-   Taxas de Churn por variáveis categóricas (output da célula com id `126a411f`), destacando a alta evasão em clientes com contrato mensal, serviço de Fibra Óptica, e métodos de pagamento como Cheque Eletrônico.
-   Distribuição de variáveis numéricas como `tenure`, `Charges.Monthly` e `Charges.Total` por Churn (box plots na célula com id `ac9793d7`), mostrando que clientes que evadem tendem a ter menor tempo de contrato e maiores cobranças mensais.
-   Matriz de correlação de variáveis numéricas, incluindo Churn (heatmap na célula com id `4ec47636`), confirmando a relação entre `tenure` e `Charges.Total` com o Churn.
-   Análise de importância das features para Regressão Logística e Random Forest (output e gráficos na célula com id `afd809cb`), identificando `tenure`, variáveis de cobrança (`Charges.Monthly`, `Charges.Total`, `Contas_Diarias`) e `InternetService_Fiber optic` como alguns dos preditores mais importantes.

## Avaliação do Modelo

Os modelos foram avaliados no conjunto de teste usando métricas como Acurácia, Precisão, Recall, F1-score, Matriz de Confusão e Curva ROC/AUC (célula com id `fd8a516f`). O modelo **Random Forest** apresentou um desempenho ligeiramente superior, sendo uma boa opção para a previsão de Churn neste caso.

## Conclusões e Recomendações

Com base na análise e modelagem, os principais fatores que influenciam a evasão e as recomendações estratégicas estão detalhados no relatório final dentro do notebook (célula com id `c6f43191`). As recomendações focam em ações direcionadas aos fatores de maior risco identificados, como clientes com contratos mensais, serviço de Fibra Óptica, altos gastos mensais e ausência de serviços adicionais.

## Como Executar o Notebook

Para replicar a análise e os modelos, siga os passos abaixo:

1.  Certifique-se de ter o Python instalado.
2.  Instale as bibliotecas necessárias executando os seguintes comandos (ou a célula correspondente no notebook, como a célula com id `0a36dc92` para `imblearn`):

# $$\color{brown}{\text{Análise Preditiva de Churn — TelecomX2}}$$

## $$\color{brown}{\text{Introdução}}$$

Este projeto tem como objetivo desenvolver modelos preditivos para identificar clientes com maior probabilidade de cancelar seus serviços (churn) na empresa Telecom X. A antecipação deste problema permite à empresa tomar decisões estratégicas para retenção de clientes e otimização de recursos.

## $$\color{brown}{\text{Metodologia}}$$

### 1. Importação e tratamento dos dados

* Os dados foram importados diretamente do repositório GitHub usando a [URL](https://raw.githubusercontent.com/LeticiaPaesano/Telecom-X-Analise-de-Churn/main/TelecomX_Data_Clean.csv)
- Realizou-se a limpeza inicial, removendo registros com valores nulos na variável alvo `Churn`.
- A variável `Churn` foi convertida para formato binário (1 para clientes que cancelaram e 0 para os que permaneceram).
- Dados numéricos originalmente em formato texto foram convertidos para formato numérico utilizando a função `pd.to_numeric()`, com padronização dos separadores decimais para ponto.
- Os dados faltantes na coluna `AvgServiceCost` foram preenchidos com a mediana calculada exclusivamente na base de treino para evitar vazamento de dados entre treino e teste.

### 2. Análise Exploratória de Dados (EDA)

- Foi calculada a proporção de clientes que apresentaram churn, correspondendo a aproximadamente **26,58%** do conjunto.
- Analisou-se a correlação entre variáveis numéricas, visualizada por meio de um mapa de calor (heatmap), para identificar relações relevantes entre atributos.

<img width="776" height="682" alt="1  Matriz de Correlação - Variáveis Numéricas" src="https://github.com/user-attachments/assets/91787410-421a-4a7e-99a2-4cdff208f0cc" />
  
- Gráficos boxplot foram utilizados para comparar a distribuição do tempo de contrato (`Tenure`) e do gasto total (`TotalCharges`) em relação ao churn, auxiliando na compreensão dos fatores associados à evasão.

<img width="686" height="470" alt="2  Tempo de Contrato vs Churn" src="https://github.com/user-attachments/assets/5a40c194-3477-488b-8b16-64b0413a89ba" />

<img width="704" height="470" alt="3  Total Gasto vs Churn" src="https://github.com/user-attachments/assets/30cb6778-48be-4e7a-a6bb-9907d770a688" />

### 3. Pré-processamento dos dados

- Variáveis categóricas foram codificadas por meio de One-Hot Encoding, convertendo-as em variáveis dummy para garantir compatibilidade com os algoritmos de machine learning.
- Dividiu-se o dataset em conjunto de treino (70%) e teste (30%), com estratificação para preservar a distribuição original da variável alvo `Churn`.
- A base de treino foi balanceada utilizando técnicas de oversampling (SMOTE) e undersampling para corrigir o desbalanceamento das classes.
- Dados foram normalizados para os modelos que requerem, tais como Regressão Logística, KNN e SVM.
- Aplicou-se redução de dimensionalidade com PCA especificamente para os modelos KNN e SVM, visando facilitar o treinamento e reduzir o risco de overfitting.

### 4. Modelagem e avaliação

- Foram treinados os seguintes modelos de classificação:

  * Regressão Logística

<img width="548" height="455" alt="4 1 Regressão Logística" src="https://github.com/user-attachments/assets/f5be8f1c-3590-48e6-9581-02969597eead" />

  * K-Nearest Neighbors (KNN)

<img width="548" height="455" alt="4 2 KNN" src="https://github.com/user-attachments/assets/589a93ec-0c2d-4800-9a22-ccb028de5c10" />

  * Support Vector Machine (SVM)

<img width="548" height="455" alt="4 3 SVM" src="https://github.com/user-attachments/assets/6d18b023-7014-404f-a760-8ad9100987e5" />

  * Random Forest

<img width="548" height="455" alt="4 4 Random Forest" src="https://github.com/user-attachments/assets/d2727e3a-0d58-406c-aeda-8c636b852ccd" />
    
  * XGBoost

<img width="548" height="455" alt="5  XGBoost" src="https://github.com/user-attachments/assets/0586c2f4-97f7-4a44-b1f9-d9ad37caf423" />

* Avaliação de desempenho com métricas: acurácia, precisão, recall, F1-score e AUC.

| Modelo              | Acurácia | Precisão | Recall | F1-Score | AUC    |
| ------------------- | -------- | -------- | ------ | -------- | ------ |
| Regressão Logística | 0,9725   | 0,9754   | 0,9198 | 0,9468   | 0,9969 |
| KNN                 | 0,7555   | 0,6860   | 0,1480 | 0,2434   | 0,7162 |
| SVM                 | 0,7967   | 0,7895   | 0,3209 | 0,4563   | 0,8673 |
| Random Forest       | 0,9891   | 0,9963   | 0,9626 | 0,9791   | 0,9999 |
| XGBoost             | 1,0000   | 1,0000   | 1,0000 | 1,0000   | 1,0000 |


- Matrizes de confusão foram visualizadas para cada modelo, facilitando a identificação dos tipos de erro (falsos positivos e falsos negativos).

### 5. Importância das variáveis

- Analisou-se a importância das variáveis utilizando o modelo Random Forest, destacando os 10 principais fatores.

<img width="1071" height="547" alt="6  Top 10 Features - Random Forest" src="https://github.com/user-attachments/assets/2e091eef-baf5-40ea-9954-0db4f2324639" />
  
- Visualização dos coeficientes da Regressão Logística, destacando variáveis com maior efeito positivo ou negativo na probabilidade de churn.

<img width="1212" height="702" alt="8  Principais Coeficientes - Regressão Logística (sem Churn_binary)" src="https://github.com/user-attachments/assets/6c4a96f3-a798-4373-8942-859fde54f222" />

### 6. Otimização do modelo Random Forest

- Realizou-se ajuste de hiperparâmetros do Random Forest por meio de RandomizedSearchCV, visando maximizar desempenho com menor complexidade computacional.
- Reavaliou-se a importância das variáveis após o ajuste, confirmando a robustez do modelo.

### 7. Exportação dos resultados

- Resultados consolidados foram exportados para arquivo Excel (`relatorio_modelos.xlsx`), contendo métricas de desempenho, predições, coeficientes e importâncias das variáveis, para análise e apresentação.

---

## $$\color{brown}{\text{Resultados principais}}$$

- O modelo Random Forest ajustado (tunado) apresentou o melhor desempenho geral, com AUC próximo ou superior a 0,99.

  <img width="1057" height="528" alt="7  Top 10 Features - Random Forest Tunado" src="https://github.com/user-attachments/assets/d2d3fa8a-e98f-4364-a406-124fd0c051f8" />
  
- Variáveis com maior impacto no churn:

  - **Tempo de contrato (Tenure):** clientes com contratos mais curtos apresentam maior risco de churn.
  - **Custo médio e total dos serviços (AvgServiceCost e TotalCharges).**
  - **Tipo de serviço de internet (InternetService_Fiber optic).**
  - **Método de pagamento (PaymentMethod_Electronic check).**

---

## $$\color{brown}{\text{Conclusão Estratégica}}$$

O modelo **Random Forest tunado** mostrou-se o mais eficaz para prever churn na Telecom X, com métricas consistentes de alta acurácia, precisão, recall e AUC. Por sua robustez e desempenho equilibrado, é o modelo recomendado para apoiar as decisões estratégicas da empresa.

As principais variáveis associadas ao churn foram:

- Tempo de contrato: contratos curtos indicam maior probabilidade de cancelamento.
- Custos médios e totais: valores elevados impactam negativamente na retenção.
- Serviço de internet por fibra óptica: apresenta maior risco de churn.
- Método de pagamento via boleto eletrônico: também correlacionado a maior evasão.

---

## $$\color{brown}{\text{Recomendações}}$$

1. Implementar campanhas específicas para clientes com **baixo tempo de contrato**, incentivando a fidelização.
2. Revisar planos e preços para clientes com **custos elevados**, buscando melhorar a percepção de valor.
3. Investir na melhoria da qualidade e confiabilidade do serviço de internet via fibra óptica.
4. Incentivar métodos de pagamento automáticos para reduzir cancelamentos relacionados a falhas no pagamento.

A aplicação contínua do modelo preditivo, combinada a essas ações, pode melhorar a retenção de clientes e otimizar os resultados financeiros da Telecom X.

---

## $$\color{brown}{\text{Limitações e Considerações Finais}}$$

- Apesar do balanceamento realizado, algum grau de desbalanceamento pode permanecer, impactando métricas como recall em alguns modelos.
- Variáveis externas e comportamentais, não disponíveis no dataset, podem influenciar o churn e devem ser consideradas em estudos futuros.
- A generalização do modelo para outras bases ou períodos deve ser validada antes de implementação operacional.

---

---

## Observação

Este repositório foi desenvolvido com o auxílio da assistente virtual ChatGPT, da OpenAI, que contribuiu para a revisão e aprimoramento do conteúdo técnico e estrutural.



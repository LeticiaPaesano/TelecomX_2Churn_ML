# $$\color{yellow}{\text{Machine Learning para Previsão de Churn de Clientes Telecom X.2}}$$

## Objetivo do Projeto
Desenvolver um modelo preditivo capaz de identificar clientes com maior probabilidade de cancelar o serviço (churn), permitindo à empresa tomar ações estratégicas de retenção e reduzir perdas financeiras.

---

## Estrutura do Projeto
- **Gráficos**: Visualizações de métricas e importância de variáveis
- **Modelos Finais**: Regressão Logística, Random Forest e XGBoost
- **README.md**: Descrição e conclusão
- **TelecomX_Data_Clean.csv**: Dataset utilizado
- **Telecom_X_2Churn_ML.ipynb**: Notebook com todo o pipeline
- **preprocessor_fitted.pkl**: Pré-processador para produção

---

## 1. Bibliotecas Principais e Justificativas
| Biblioteca | Uso no Projeto | Justificativa |
|------------|----------------|---------------|
| `pandas` & `numpy` | Manipulação e análise de dados | Tratamento de datasets, cálculos e operações vetorizadas |
| `matplotlib` & `seaborn` | Visualização | Criação de gráficos para análise exploratória e métricas de modelos |
| `scikit-learn` | Modelos e pré-processamento | Modelos clássicos (Logistic Regression, Random Forest), pipelines, métricas e cross-validation |
| `xgboost` | Modelos de alto desempenho | Algoritmo eficiente para classificação com boa generalização e tratamento de variáveis categóricas |
| `imblearn (SMOTE)` | Balanceamento de classes | Corrige desbalanceamento, evitando que o modelo aprenda apenas a classe majoritária |
| `joblib` | Persistência de modelos | Salvar pré-processador e modelos treinados para produção |

**Justificativa geral**: As bibliotecas escolhidas permitem construir um pipeline completo de machine learning, desde pré-processamento até avaliação e produção.

---

## 2. Pré-processamento dos Dados
- Remoção de colunas duplicadas ou irrelevantes (`Churn_binary`)  
- Conversão de colunas numéricas (`DailyCharges`, `MonthlyCharges`, `TotalCharges`, `AvgServiceCost`)  
- Criação de feature derivada: `AvgMonthlyCost = TotalCharges / Tenure`  
- Conversão do alvo `Churn` em binário (`1 = cancelou`, `0 = manteve`)  

**Por que feito**:  
Evita vazamento de dados, garante consistência nos tipos numéricos e cria informações adicionais que capturam comportamento financeiro.

---

## 3. Divisão Treino / Teste
- Treino: 70%, Teste: 30%  
- Uso de `stratify=y` para manter proporção de churn igual  

**Por que feito**:  
Mantém representatividade do churn em ambos conjuntos, permitindo avaliação realista do modelo.

---

## 4. Pré-processamento com Pipeline
- **Colunas numéricas**: mantidas como estão no dataset  
- **Colunas categóricas**: One-Hot Encoding (`drop='first`)  

**Por que feito**:  
Garante que variáveis categóricas sejam transformadas em formato numérico compatível com modelos de ML, evitando multicolinearidade e facilitando o aprendizado.  
As colunas numéricas já estavam consistentes, então não foi necessário aplicar imputação ou normalização.

---

## 5. Balanceamento de Classes
- Aplicação de **SMOTE**   

**Problema resolvido**:  
Evita viés do modelo para a classe majoritária (clientes que não cancelam), melhorando recall da classe minoritária.

---

## 6. Modelos Avaliados e Justificativas
| Modelo | Por que escolhido | Observação |
|--------|-----------------|------------|
| Regressão Logística | Interpretável, permite identificar importância de features | Ideal para explicar resultados para área de negócios |
| Random Forest | Robustez, boa performance com dados desbalanceados | Reduz overfitting em relação a modelos simples |
| XGBoost | Alta performance, otimizado para classificação | Tratamento eficiente de variáveis categóricas e missing values |

**Estratégia**: Comparar modelos simples (explicáveis) e complexos (alto desempenho) para selecionar o melhor equilíbrio entre performance e interpretabilidade.

---

## 7. Avaliação de Modelos
Métricas utilizadas:
- Acurácia
- Precisão
- Recall
- F1-Score
- AUC (Área sob a curva ROC)

<img width="548" height="455" alt="1  Regressão Logistica" src="https://github.com/user-attachments/assets/cdb48799-f5cb-4982-b3cd-73cc61280d61" />

<img width="548" height="455" alt="2  Random Forest" src="https://github.com/user-attachments/assets/96f6eaca-73a5-4ed2-94cb-9e6f4f91d414" />

<img width="548" height="455" alt="3  XGBoost" src="https://github.com/user-attachments/assets/a7b80f4c-c0a2-4ef5-8db9-1ac4399d336b" />

**Por que feito**:  
Para escolher o modelo mais adequado, priorizamos **Recall e AUC**, essenciais em churn, pois identificar clientes em risco é mais importante que falsos positivos.

**Resultados resumidos**:

| Modelo               | Acurácia | Precisão | Recall | F1-Score | AUC   |
|----------------------|----------|----------|--------|----------|-------|
| Regressão Logística  | 0.74     | 0.51     | 0.79   | 0.62     | 0.84  |
| Random Forest        | 0.77     | 0.58     | 0.56   | 0.57     | 0.82  |
| XGBoost              | 0.77     | 0.58     | 0.54   | 0.56     | 0.81  |

---

## 8. Análise de Importância de Variáveis
- Variáveis com maior impacto no churn:  
  `AvgMonthlyCost`, `Tenure`, `Contract`, `FiberOpticService`, `SeniorCitizen`  

<img width="1057" height="548" alt="4  Top 20 Features - Regressão Logística" src="https://github.com/user-attachments/assets/f0ec0790-dde0-4ac5-a52f-808dadadd575" />

<img width="1057" height="548" alt="5  Top 10 Features - Random Forest" src="https://github.com/user-attachments/assets/2ed7b907-78a7-467b-b14e-6adf620f7d91" />

<img width="1057" height="548" alt="6  Top 10 Features - XGBoost" src="https://github.com/user-attachments/assets/fb4d0070-a9cc-4516-b8a2-3325bb453540" />

**Por que feito**:  
Permite à empresa entender os fatores que mais influenciam evasão e direcionar ações estratégicas.

---

## 9. Curvas ROC
- Comparação visual dos modelos usando **ROC e AUC**  
- ROC mostra trade-off entre **recall** e **FPR**  
- AUC indica capacidade de separação do modelo (quanto mais próximo de 1, melhor)

<img width="691" height="547" alt="7  Curva ROC - Comparação de Modelos" src="https://github.com/user-attachments/assets/f53c045a-79ca-4654-b83b-39885d768e6d" />

**Por que feito**:  
Auxilia na escolha do melhor modelo para produção.

---

## 10. Ajuste de Threshold
- Alteração do threshold da Regressão Logística para atingir **recall ≥ 0.8**  

**Por que feito**:  
Priorizar recall é fundamental para problemas de churn, garantindo que o maior número possível de clientes em risco seja identificado.

---

## 11. Modelo Final e Artefatos do Repositório
- **Melhor modelo escolhido**: Regressão Logística  
- **Artefatos salvos**:  
  - `preprocessor_fitted.pkl`  
  - `modelo_logistica_final.pkl`  
  - `modelo_rf_final.pkl`  
  - `modelo_xgb_final.pkl`  

**Por que feito**:  
Modelos e pré-processador persistidos para produção; SMOTE não é salvo, pois foi usado apenas para treino.

---

## 12. Estratégias de Retenção Sugeridas
- Acompanhamento de clientes novos nos primeiros meses  
- Investigar causas de evasão em clientes de fibra ótica  
- Ofertas e suporte para clientes idosos  
- Campanhas de fidelização com contratos de longo prazo  

**Por que feito**:  
Baseado em features mais importantes, estas ações visam reduzir churn e aumentar receita.

---

## 13. Próximos Passos
- Implementar modelo em produção e monitorar desempenho  
- Atualizar dados periodicamente e re-treinar modelo  
- Avaliar impacto das ações de retenção

---
  
## Observação

Este repositório foi desenvolvido com o auxílio da assistente virtual ChatGPT, da OpenAI, que contribuiu para a revisão e aprimoramento do conteúdo técnico e estrutural.



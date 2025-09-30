# Documentação do Código – Pipeline Ultra Otimizado

##  Objetivo
Esse código implementa um **pipeline avançado de Machine Learning** para prever o **sucesso ou insucesso de startups**.  

Ele inclui:
-  Engenharia de features (criação de novas variáveis informativas)  
-  Pré-processamento avançado (tratamento de nulos, escalonamento, encoding categórico)  
-  Balanceamento de classes com **SMOTE**  
-  Treinamento de modelos de ponta (**Random Forest, LightGBM, XGBoost**)  
-  Combinação dos modelos via **Stacking Classifier**  
-  Otimização do **threshold** para maximizar acurácia acima de **80%**  
-  Geração do arquivo final de submissão para competição  

---

##  Estrutura do Pipeline

### 1. Importação de bibliotecas
Bibliotecas para manipulação de dados, aprendizado de máquina, balanceamento de classes, métricas e visualização.

### 2. Carregamento dos dados
- `train.csv` → dataset de treino.  
- `test.csv` → dataset de teste.  
- `sample_submission.csv` → arquivo base para submissão.  

### 3. Análise inicial
- Exibe **shape** dos datasets.  
- Mostra **distribuição da variável alvo** (`labels`).  
- Verifica valores ausentes.  

---

### 4. Pré-processamento
- Tratamento de colunas de idade (`age_first_funding_year`, etc.) → imputação com mediana.  
- Preenchimento de valores nulos em `funding_total_usd`.  
- Substituição de **infinito** por `NaN` e preenchimento final com `0`.  

---

### 5. Engenharia de Features
Criação de variáveis derivadas para enriquecer o modelo:
- `funding_per_round` → financiamento por rodada  
- `milestones_per_year` → marcos por ano  
- `has_multiple_rounds` → flag de startups com mais de uma rodada  
- `funding_age_span` → diferença entre primeira e última rodada  
- `relationships_per_milestone` → relações por marco  
- `funding_efficiency` → financiamento dividido por relações  
- `milestone_density` → densidade de marcos por rodada  
- `avg_funding_per_participant` → financiamento médio por participante  
- `funding_milestone_ratio` → relação entre financiamento e marcos  

Essas features ajudam os modelos a capturar **padrões mais complexos** de sucesso.

---

### 6. Encoding e Scaling
- **One-Hot Encoding** (`pd.get_dummies`) para `category_code`.  
- **StandardScaler** nos atributos numéricos (normalização para melhor performance).  

---

### 7. Divisão dos dados
- `train_test_split` → 80% treino / 20% validação, estratificado pela variável alvo.  

---

### 8. Balanceamento das classes
- Uso de **SMOTE** para gerar exemplos sintéticos da classe minoritária.  
- Reduz o desbalanceamento e melhora a generalização dos modelos.  

---

### 9. Modelos individuais
Treinamento de três algoritmos potentes:
-  **Random Forest** (ensemble de árvores)  
-  **LightGBM** (boosting otimizado, rápido e eficiente)  
-  **XGBoost** (boosting de alta performance)  

---

### 10. Stacking Classifier
- Combina as previsões dos três modelos base.  
- **Meta-modelo:** Logistic Regression.  
- `passthrough=True` → o meta-modelo também recebe as features originais.  

---

### 11. Threshold Tuning
- Avalia thresholds de **0.30 a 0.80** (passo de 0.001).  
- Seleciona o threshold que gera **melhor acurácia** na validação.  
- Ajusta a sensibilidade do modelo além do threshold padrão (0.5).  

---

### 12. Avaliação
Métricas utilizadas:
-  **Accuracy** (acurácia)  
-  **AUC** (área sob a curva ROC)  
-  **Classification Report** 

---

### 13. Treinamento final
- Reaplica **SMOTE** em todo o dataset de treino.  
- Re-treina o **Stacking Classifier** nos dados completos.  
- Gera previsões para o conjunto de teste.  

---

### 14. Geração do arquivo de submissão
- Salva o resultado em **`my_submission_ultra_optimized.csv`** com as colunas:  
  - `id`  
  - `labels` 
---

##  Resumo Final impresso no console
-  Total de **novas features criadas**  
-  Método de **balanceamento**: SMOTE  
- Modelos: **Random Forest + LightGBM + XGBoost**  
-  Meta-modelo: **Stacking Classifier**  
-  Threshold otimizado  
-  Acurácia final e AUC  
-  Distribuição das predições (classe 0 vs classe 1)  

---

##  Conclusão
Esse pipeline é **robusto e escalável**, combinando:  
- Engenharia de features inteligentes  
- Modelos de última geração  
- Ensemble via Stacking  
- Balanceamento de classes  
- Threshold tuning fino 

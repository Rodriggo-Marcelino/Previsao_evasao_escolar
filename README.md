# 🎓 Previsão de Evasão Escolar com Machine Learning

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0-green.svg)](https://pandas.pydata.org/)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.3-orange.svg)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-1.7-red.svg)](https://xgboost.readthedocs.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Repositório oficial do Trabalho de Conclusão de Curso (TCC) em Ciência da Computação. Este projeto desenvolve um pipeline completo de dados e aplica modelos de **Machine Learning** para prever a evasão escolar no Brasil, utilizando microdados públicos do Censo Escolar e informações socioeconômicas.

---

## 📌 Índice

- [Visão Geral](#-visão-geral)
- [O Problema da Evasão](#-o-problema-da-evasão)
- [Arquitetura do Pipeline (Medallion)](#-arquitetura-do-pipeline-medallion)
- [Estrutura do Repositório](#-estrutura-do-repositório)
- [Metodologia de Modelagem](#-metodologia-de-modelagem)
- [Resultados e Análises](#-resultados-e-análises)
- [Interpretabilidade com SHAP](#-interpretabilidade-com-shap)
- [Trabalhos Futuros](#-trabalhos-futuros)
- [Como Executar](#-como-executar)
- [Autor](#-autor)

---

## 🔍 Visão Geral

O projeto integra e processa os microdados do Censo Escolar do INEP, enriquece‑os com indicadores socioeconômicos (IBGE) e aplica uma ampla gama de algoritmos de classificação para identificar estudantes com alto risco de evasão. O objetivo é fornecer uma ferramenta baseada em evidências para que gestores educacionais possam intervir precocemente.

**Principais etapas:**
- Extração, carga e transformação (ELT) dos dados.
- Enriquecimento com dados de escolas, docentes, gestores e municípios.
- Análise exploratória (EDA) e preparação dos dados.
- Treinamento e comparação de 10 modelos de Machine Learning.
- Avaliação robusta com validação cruzada estratificada.
- Interpretabilidade dos resultados via SHAP.

---

## ⚠️ O Problema da Evasão

A evasão escolar é um grave desafio da educação brasileira, com impactos profundos:
- Aumento da desigualdade social.
- Redução da empregabilidade e da renda futura.
- Perpetuação de ciclos de pobreza.

A aplicação de **Ciência de Dados** neste contexto permite:
- Identificar padrões não‑óbvios associados ao abandono.
- Criar sistemas de alerta precoce para alunos em risco.
- Otimizar a alocação de recursos e políticas públicas.

---

## 🏗️ Arquitetura do Pipeline (Medallion)

A organização dos dados segue a arquitetura Medallion, garantindo qualidade, rastreabilidade e prontidão para análise.

### 🥉 Camada Bronze
Dados brutos, exatamente como extraídos das fontes oficiais:
- `microdados_matricula.parquet`
- `microdados_escola.parquet`
- `microdados_docente.parquet`
- `microdados_gestor.parquet`
- `microdados_turma.parquet`

### 🥈 Camada Silver
Dados limpos, padronizados e integrados. As tabelas são combinadas para criar uma visão unificada por escola:
- `escola_enriquecida.parquet` (junção com dados de municípios, docentes e gestores)

### 🥇 Camada Gold
Dataset analítico final, pronto para modelagem. Contém a variável‑alvo (evasão) e todas as *features* derivadas:
- `gold_escolas_evasao.parquet`

---

## 📂 Estrutura do Repositório
```
tcc-evasao-escolar/
│
├── data/
│   ├── raw/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── notebooks/
│   ├── 01_elt_pipeline.ipynb
│   ├── 02_eda.ipynb
│   └── 03_modelagem.ipynb
│
├── src/
│   └── utils.py
│
├── figures/
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

## 🤖 Metodologia de Modelagem

### 1. Definição da Variável‑Alvo
A variável `target_evasao` é definida como **1** se o aluno não possui matrícula no ano seguinte, e **0** caso contrário.

### 2. Pré‑processamento
- Remoção de *features* usadas na criação do *target* para evitar *leakage*.
- Imputação de valores ausentes (mediana).
- Normalização (`StandardScaler`) para modelos sensíveis à escala.
- **Balanceamento** da classe majoritária via **undersampling** (razão 1:1 entre classes).

### 3. Modelos Avaliados
Foram testados 10 algoritmos de classificação, com ajuste de hiperparâmetros via `RandomizedSearchCV` (otimizando o F1‑Score):

| Modelo                | Escala Necessária? |
|-----------------------|--------------------|
| Regressão Logística   | Sim                |
| K‑Nearest Neighbors   | Sim                |
| Decision Tree         | Não                 |
| Random Forest         | Não                 |
| Gradient Boosting     | Não                 |
| XGBoost               | Não                 |
| LightGBM              | Não                 |
| MLP (Rede Neural)     | Sim                 |
| SVM (SVC)             | Sim                 |
| CatBoost              | Não                 |

### 4. Avaliação
- **Validação Cruzada Estratificada** (5 folds) no conjunto de treino balanceado.
- **Conjunto de Teste** independente (20% dos dados originais).
- Métricas: Acurácia, Precisão, Recall, F1‑Score, AUC‑ROC, MCC, Kappa e Acurácia Balanceada.

---

## 📈 Resultados e Análises

### Desempenho dos Modelos no Conjunto de Teste

| Modelo               | Acurácia | Precisão | Recall | F1‑Score | AUC‑ROC | MCC   |
|----------------------|----------|----------|--------|----------|---------|-------|
| Random Forest        | 0.5916   | 0.3781   | 0.7761 | **0.5085**| 0.7175  | 0.2677|
| LightGBM             | 0.5653   | 0.3630   | 0.7908 | 0.4976   | 0.6938  | 0.2463|
| XGBoost              | 0.5649   | 0.3630   | 0.7926 | 0.4979   | 0.6948  | 0.2469|
| MLP                  | 0.6166   | 0.3911   | 0.7336 | 0.5102   | 0.7111  | 0.2729|
| SVC                  | 0.5554   | 0.3574   | 0.7939 | 0.4929   | 0.6770  | 0.2368|
| CatBoost             | 0.5803   | 0.3712   | 0.7805 | 0.5031   | 0.7046  | 0.2572|
| Decision Tree        | 0.5712   | 0.3636   | 0.7668 | 0.4933   | 0.6590  | 0.2384|
| Regressão Logística  | 0.5460   | 0.3425   | 0.7260 | 0.4654   | 0.6336  | 0.1844|
| KNN                  | 0.6905   | 0.4120   | 0.3207 | 0.3606   | 0.5837  | 0.1628|

> **Destaques:**
> - **Random Forest** obteve o melhor equilíbrio entre Recall (76,6%) e F1‑Score (0,5085), sendo o modelo escolhido para análise final.
> - **MLP** apresentou a maior acurácia (61,7%) e o segundo melhor F1‑Score (0,5102).
> - **XGBoost** e **LightGBM** também apresentaram bom desempenho, com recalls elevados (~79%).

### Importância das Variáveis (Random Forest)
As *features* mais relevantes para a predição da evasão foram:
1. `QT_DOC_BAS_MASC` – Número de docentes do sexo masculino na educação básica.
2. `QT_DOC_BAS_FEM` – Número de docentes do sexo feminino.
3. `QT_TUR_MED` – Total de turmas de ensino médio.
4. `QT_GESTORES` – Quantidade de gestores escolares.
5. `tem_ensino_medio` – Indicador se a escola oferece ensino médio.

Esses resultados sugerem que a **composição do corpo docente e a estrutura de turmas** são fatores centrais na evasão, mais do que variáveis geográficas ou de infraestrutura básica.

---

## 🔬 Interpretabilidade com SHAP

Para o modelo **LightGBM**, foram gerados gráficos SHAP (SHapley Additive exPlanations) que revelam:
- O aumento do número de docentes masculinos (`QT_DOC_BAS_MASC`) está associado a **maior risco de evasão**.
- Escolas com **ensino médio** apresentam maior probabilidade de evasão.
- Turmas de ensino médio (`QT_TUR_MED`) também contribuem positivamente para o risco.

Esses insights são valiosos para a formulação de políticas: intervenções podem ser direcionadas a escolas com perfis docentes específicos ou com oferta de ensino médio.


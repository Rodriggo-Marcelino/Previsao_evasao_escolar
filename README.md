# 🎓 Previsão de Evasão Escolar com Machine Learning

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0-green.svg)](https://pandas.pydata.org/)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.3-orange.svg)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Este é o repositório oficial do meu Trabalho de Conclusão de Curso (TCC) em Ciência da Computação. O projeto consiste no desenvolvimento de um pipeline completo de dados e na aplicação de modelos de **Machine Learning** para a predição da evasão escolar no Brasil, utilizando dados públicos oficiais.

![Evasão Escolar](https://via.placeholder.com/800x200.png?text=Previsão+de+Evasão+Escolar+com+Machine+Learning)

## 📝 Índice

- [Visão Geral do Projeto](#-visão-geral-do-projeto)
- [O Problema da Evasão](#-o-problema-da-evasão)
- [Arquitetura do Pipeline (Medallion)](#-arquitetura-do-pipeline-medallion)
- [Estrutura do Repositório](#-estrutura-do-repositório)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Modelagem Preditiva](#-modelagem-preditiva)
- [Resultados e Análises](#-resultados-e-análises)
- [Trabalhos Futuros](#-trabalhos-futuros)
- [Como Executar](#-como-executar)
- [Autor](#-autor)

## 🔍 Visão Geral do Projeto

O objetivo principal deste trabalho é construir uma solução baseada em dados capaz de identificar, com antecedência, alunos com alto risco de evasão escolar. Para isso, o projeto integra e processa os microdados do Censo Escolar, enriquecendo-os com informações socioeconômicas externas, a fim de treinar e avaliar modelos preditivos.

**Principais etapas do projeto:**

1.  **Extração, Carga e Transformação (ELT):** Importação e tratamento dos dados brutos do INEP.
2.  **Enriquecimento de Dados:** Junção com dados socioeconômicos do IBGE e outras fontes.
3.  **Análise Exploratória (EDA):** Investigação de padrões, correlações e fatores associados à evasão.
4.  **Modelagem:** Treinamento e comparação de múltiplos algoritmos de classificação.
5.  **Avaliação:** Análise de métricas e identificação das variáveis mais importantes para o fenômeno.

## ⚠️ O Problema da Evasão

A evasão escolar é um dos maiores desafios da educação brasileira, com profundas consequências sociais e econômicas, como:

- Aumento da desigualdade social.
- Redução da empregabilidade e da renda futura.
- Perpetuação de ciclos de pobreza.

A aplicação de **Ciência de Dados** neste contexto visa fornecer ferramentas para que gestores educacionais possam agir de forma proativa, direcionando recursos e políticas públicas para os alunos e escolas que mais precisam.

## 🏗️ Arquitetura do Pipeline (Medallion)

O projeto adota a arquitetura Medallion para organizar os dados em camadas, garantindo qualidade e rastreabilidade.

### 🥉 Camada Bronze
Nesta camada, os dados são ingeridos exatamente como estão nas fontes oficiais.
- `microdados_matricula.parquet`
- `microdados_escola.parquet`
- `microdados_docente.parquet`
- `microdados_gestor.parquet`
- `microdados_turma.parquet`

### 🥈 Camada Silver
Os dados da camada Bronze são limpos, padronizados e enriquecidos. Aqui, as tabelas são unidas para criar uma visão mais coesa da realidade escolar.
- `escola_enriquecida.parquet` (Escolas + Municípios + Docentes + Gestores)

### 🥇 Camada Gold
A camada final, pronta para consumo. Os dados da matrícula são combinados com as informações da escola enriquecida e os dados socioeconômicos, gerando o **dataset analítico** para a modelagem.
- `dataset_evasao.parquet` (Dataset final com a variável alvo e todas as features)

## 📁 Estrutura do Repositório
tcc-evasao-escolar/
│
├── data/ # Diretório para dados (ignorado pelo git)
│ ├── raw/ # Dados originais (fonte primária)
│ ├── bronze/ # Dados brutos carregados
│ ├── silver/ # Dados tratados e relacionados
│ └── gold/ # Dataset final para Machine Learning
│
├── notebooks/ # Jupyter Notebooks com todo o processo
│ ├── 01_elt_pipeline.ipynb # Pipeline de Extração, Carga e Transformação
│ ├── 02_eda.ipynb # Análise Exploratória dos Dados
│ └── 03_modelagem.ipynb # Treinamento, avaliação e seleção de modelos
│
├── src/ # Código-fonte reutilizável
│ └── utils.py # Funções auxiliares (ex: carregamento de dados)
│
├── .gitignore # Arquivos e pastas ignorados pelo Git
├── README.md # Documentação principal do projeto (este arquivo)
└── requirements.txt # Dependências do projeto


## 🛠️ Tecnologias Utilizadas

- **Linguagem:** Python 3.8+
- **Manipulação de Dados:** Pandas, NumPy
- **Armazenamento:** Parquet (para eficiência e performance)
- **Visualização:** Matplotlib, Seaborn
- **Machine Learning:** Scikit-learn, XGBoost
- **Ambiente de Desenvolvimento:** Jupyter Notebook, VS Code
- **Controle de Versão:** Git

## 🤖 Modelagem Preditiva

A variável alvo (evasão) é definida com base na situação do aluno no ano seguinte (ex: se não há matrícula subsequente). Os modelos testados incluem:

- **Regressão Logística:** Modelo baseline para classificação binária.
- **Random Forest:** Algoritmo de ensemble baseado em árvores de decisão, robusto e capaz de capturar relações não-lineares.
- **XGBoost:** Técnica de gradient boosting conhecida por sua alta performance e precisão.

A avaliação dos modelos será feita com foco em **Recall (Sensibilidade)** e **F1-Score**, pois é mais crítico identificar corretamente os alunos que irão evadir (falsos negativos são mais custosos do que falsos positivos). As principais métricas calculadas serão:

- Acurácia (Accuracy)
- Precisão (Precision)
- **Recall (Sensibilidade)**
- **F1-Score**
- Curva ROC e AUC

## 📈 Resultados e Análises

*(Esta seção será atualizada com os resultados finais da pesquisa)*

As análises exploratórias iniciais visam responder perguntas como:

- Qual a correlação entre a infraestrutura da escola e as taxas de evasão?
- Como fatores socioeconômicos do município (PIB per capita, IDH) impactam o abandono escolar?
- Existem diferenças significativas nas taxas de evasão entre redes de ensino (pública x privada)?

Os resultados da modelagem indicarão qual algoritmo apresentou o melhor desempenho preditivo e quais *features* (características) são os maiores preditores da evasão.

## 🚀 Trabalhos Futuros

Este projeto serve como base para diversas extensões, tais como:

- **Análise Temporal:** Estudar a evolução da evasão ao longo dos anos.
- **Deploy do Modelo:** Criar uma API simples para que o modelo possa ser utilizado por sistemas educacionais.
- **Dashboard Interativo:** Desenvolver um dashboard (ex: com Power BI ou Streamlit) para visualização dos principais indicadores e predições.
- **Integração com Novas Fontes:** Incorporar dados de programas sociais (ex: Bolsa Família) para enriquecer ainda mais a análise.

## ▶️ Como Executar

1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/RodrigoMarcelino/tcc-evasao-escolar.git
    cd tcc-evasao-escolar

2. **Crie e ative um ambiente virtual (recomendado)**
    ```bash
    python -m venv venv
    source venv/bin/activate  # No Windows: venv\Scripts\activate   

3. **Instale as dependências:**
     ```bash
    pip install -r requirements.txt 

4. **Baixe os dados: Coloque os microdados do Censo Escolar (formato CSV) na pasta data/raw/. (Instruções de download podem ser encontradas no site do INEP).**

5. **Execute os notebooks: Abra e execute os notebooks na ordem numérica (01_, 02_, 03_) para reproduzir todo o pipeline.**
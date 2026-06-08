# Sistema Inteligente de Monitoramento da Qualidade do Ar

> **Projeto Prático da Disciplina de Algoritmos de Inteligência Artificial (Classificadores)** > **Instituição:** UNIMAR
> 
> **Link da Aplicação Web:** [Acesse o App Operacional no Streamlit Cloud](https://previsao-de-qualidade-do-ar-htr3agljavthx5ppztq7su.streamlit.app/)

---

## Integrantes e RAs (Grupo 6)
* **Felipe Augusto Santos Dorta de Oliveira** — RA: 2052799
* **Bruno Marcelo Rocha da Silva** — RA: 1988442
* **Lucas de Azevedo Kopeska Paraizo** — RA: 2054984

---
## Tecnologias Utilizadas

- **Linguagem:** Python (v3.10+)
- **Manipulação e Engenharia de Dados:** pandas, numpy
- **Modelagem e Machine Learning:** scikit-learn
- **Serialização de Artefatos:** joblib
- **Interface Gráfica e Deploy Cloud:** streamlit e Streamlit Community Cloud

---
## Como Executar o Projeto Localmente

## Passo 1: Clonar o Repositório

Abra o terminal e execute:
```bash
git clone https://github.com/Lucas-Kopeska/PREVISAO-DE-QUALIDADE-DO-AR.git
cd PREVISAO-DE-QUALIDADE-DO-AR
```

## Passo 2: Instalar as Dependências
Recomenda-se utilizar um ambiente virtual (`venv`).
Instale os módulos necessários via `pip`:
```bash
pip install -r requirements.txt
```
---
## Instruções para Executar o Notebook
Certifique-se de ter o Jupyter Notebook ou Jupyter Lab instalado:
```bash
pip install jupyter
```
Inicialize o ambiente:
```bash
jupyter notebook
```
Abra o arquivo:
```text
Trabalho_Qualidade_do_Ar_(1).ipynb
```
Para reexecutar toda a pipeline e gerar novos arquivos de modelo:
1. Clique em **Kernel**
2. Selecione **Restart & Run All**
---

## Instruções para Executar o App Streamlit
Com as dependências instaladas na raiz do repositório, execute:
```bash
streamlit run app.py
```
O Streamlit abrirá automaticamente uma nova aba no navegador padrão no endereço:
```text
http://localhost:8501
```
---

## Descrição do Problema
A poluição atmosférica nos grandes centros urbanos é um dos desafios mais severos à saúde pública contemporânea. Gases nocivos provenientes da queima de combustíveis fósseis e de atividades industriais causam impactos imediatos e de longo prazo na saúde de grupos de risco (crianças, idosos e cardiopatas). 

Contudo, os sensores físicos industriais utilizados para monitorar estes poluentes frequentemente enfrentam instabilidades, falhas ou necessitam de validação laboratorial demorada. O desafio reside em interpretar essa matriz complexa de sensores físico-químicos e dados climáticos para emitir alertas rápidos e confiáveis sobre a salubridade do ar.

## Objetivo do Projeto
Desenvolver um ecossistema de Inteligência Artificial capaz de processar, tratar e interpretar as leituras de sensores ambientais para prever e classificar, em tempo real, se a qualidade geral do ar atmosférico urbano está **"Boa" (0)** ou **"Ruim" (1)**. O foco central é entregar um classificador de alta generalização implantado em ambiente web.

## Dataset Utilizado
O projeto baseia-se no dataset internacional **Air Quality Data Set**, hospedado no UCI Machine Learning Repository e no [Kaggle](https://www.kaggle.com/datasets/fedesoriano/air-quality-data-set). 

O conjunto de dados contém 9.358 respostas horárias de uma matriz de 5 sensores químicos de óxidos metálicos implantados em uma área significativamente poluída na Itália, cobrindo o período de Março de 2004 a Fevereiro de 2005.
* **Valores Ausentes:** O dataset original indica ausência de dados laboratoriais através do valor `-200`. Estas ocorrências foram devidamente mapeadas e tratadas via imputação estatística e engenharia de dados.

## Tipo de Problema de Machine Learning
Este é um problema clássico de **Aprendizado Supervisionado** com foco em **Classificação Binária**. O modelo recebe uma sequência de variáveis contínuas (sensores) e discretas (tempo/clima) para mapear a fronteira de decisão entre duas classes exclusivas:
* **Classe 0:** Qualidade do Ar Boa
* **Classe 1:** Qualidade do Ar Ruim (Alerta de Poluição)

---

## Metodologia

O pipeline de ML de classificação foi estruturado seguindo as melhores práticas acadêmicas e de engenharia:
1. **Análise Exploratória de Dados (EDA):** Identificação de distribuições, detecção de outliers e mapeamento de anomalias (como o valor `-200`).
2. **Pré-Processamento e Imputação:** Substituição de ruídos por técnicas robustas e binarização da variável alvo com base nos limites críticos de monóxido de carbono ($CO$).
3. **Engenharia de Features Sazonais:** Extração de padrões temporais ricos como o `mês`, `dia_semana` e a `hora` exata da medição para capturar dinâmicas de tráfego veicular e inversão térmica.
4. **Padronização Estatística:** Aplicação de `StandardScaler` para garantir que modelos baseados em distâncias (como o KNN) ou pesos não fossem influenciados por escalas numéricas distintas.
5. **Ajuste Combinatório de Hiperparâmetros:** Utilização de `GridSearchCV` acoplado com **Validação Cruzada Estratificada ($K=5$)** para mitigar riscos de *overfitting* e *underfitting*.

---

## Modelos Treinados
Ao longo da evolução do projeto (fases P1 e P2), três arquiteturas distintas foram avaliadas no conjunto de teste:
1. **K-Nearest Neighbors (KNN):** Utilizado como classificador geométrico de base (*Baseline*).
2. **Gradient Boosting Classifier:** Algoritmo de *ensemble* baseado em árvores sequenciais.
3. **Random Forest Classifier (Base):** Configuração nativa sem restrição de crescimento (P1), apresentando forte cenário de *overfitting* no treino.
4. **Random Forest Classifier (Otimizado):** Versão submetida à malha de sintonia fina do GridSearch (P2) para calibração de profundidade e número de estimadores.

## Modelo Final Escolhido
O modelo definitivo para o deploy em produção foi o **Random Forest Classifier Otimizado**. Ele foi selecionado por apresentar o melhor equilíbrio estatístico, estabilidade macroclimática e ausência de viés de memorização. Os hiperparâmetros finais obtidos pelo GridSearch foram balanceados para garantir robustez: `max_depth: 20`, `min_samples_split: 2`, `min_samples_leaf: 1` e `n_estimators: 200`.

---

## Métricas de Avaliação e Principais Resultados

A tabela abaixo exibe o comportamento dos algoritmos sobre o conjunto de dados de teste isolado e inédito:

| Classificador / Modelo | Acurácia | Precisão | Recall | F1-Score | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| K-Nearest Neighbors (KNN) | 0.9194 | 0.9253 | 0.9029 | 0.9140 | 0.9753 |
| Gradient Boosting | 0.9309 | 0.9233 | 0.9317 | 0.9275 | 0.9827 |
| Random Forest (Base - P1) | 0.9370 | 0.9370 | 0.9290 | 0.9330 | 0.9850 |
| **Random Forest (Otimizado - P2)** | **0.9395** | **0.9363** | **0.9363** | **0.9363** | **0.9856** |

### Destaques dos Resultados:
* **F1-Score de 0.9363:** Média harmônica ideal que comprova o excelente equilíbrio do modelo entre evitar alarmes falsos (Precisão) e não deixar de detectar ar poluído (Recall).
* **AUC-ROC de 0.9856:** Demonstra uma probabilidade de 98,56% do modelo discriminar corretamente uma amostra de ar poluído de uma amostra limpa.
* **Interpretabilidade (SHAP Values & MDI):** Revelou que as variáveis mais críticas para as divisões das ramificações das árvores são o sensor químico `PT08.S1(CO)` e as concentrações laboratoriais de Óxidos de Nitrogênio (`NOx(GT)`), validando empiricamente a lógica de negócios e química atmosférica urbana.

---
## Limitações do Modelo

# Janela Temporal Fixa
O modelo foi treinado com dados coletados entre **2004 e 2005 na Europa**. Mudanças significativas na composição da frota moderna, como o crescimento de veículos elétricos ou novas regulamentações de emissão (Proconve/Euro), podem reduzir a capacidade de generalização do sistema e exigir retreinamento.

# Dependência de Sensores de Co-localização
O modelo requer que todos os dados provenientes dos sensores físicos estejam disponíveis durante a inferência. A falha ou ausência de sensores críticos (como o **PT08.S1**) compromete a cadeia de predição, caso não exista um mecanismo de contingência para substituição ou imputação dos dados.

---
## Conclusão
O projeto atingiu com êxito os objetivos estipulados para a fase **P2**. A evolução de um modelo inicialmente suscetível ao sobreajuste para um classificador otimizado e calibrado por meio de **GridSearchCV** demonstrou a importância do rigor estatístico no desenvolvimento de aplicações industriais de Inteligência Artificial.
Com métricas consolidadas superiores a **93% de F1-Score** e uma aplicação funcional de baixa latência implantada no **Streamlit Community Cloud**, o sistema demonstra um esboço de viabilidade prática para apoiar políticas de saúde pública, permitindo que cidadãos e gestores municipais tomem decisões preditivas relacionadas aos riscos da poluição atmosférica urbana.

## Estrutura dos Arquivos do Repositório
```text
PREVISAO-DE-QUALIDADE-DO-AR/
├── AirQuality.csv                    # Dataset original com os registros ambientais
├── Trabalho_Qualidade_do_Ar_(1).ipynb # Notebook Jupyter com o pipeline de ciência de dados
├── app.py                            # Código-fonte da aplicação interativa em Streamlit
├── requirements.txt                  # Arquivo de dependências e bibliotecas do projeto
├── modelo_qualidade_ar.pkl           # Pesos salvos do Random Forest Otimizado (Joblib)
├── scaler_qualidade_ar.pkl           # Normalizador estatístico salvo (StandardScaler)
└── README.md                         # Documentação oficial do repositório

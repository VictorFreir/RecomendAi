# RecomendAI: Sistema Híbrido de Recomendação de Filmes

## 1. Resumo Executivo

O **RecomendAI** é um protótipo de um sistema de recomendação de filmes desenvolvido como projeto para a disciplina de Inteligência Artificial. O objetivo principal é fornecer recomendações personalizadas (Top-N) para usuários, utilizando uma abordagem híbrida que combina a análise do histórico de avaliações com a análise semântica do conteúdo dos filmes.

O sistema foi implementado em um Notebook Python no Google Colab e utiliza o dataset **MovieLens 100k**. A solução emprega uma rede neural para a recomendação inicial e um modelo de Processamento de Linguagem Natural (PLN) para uma segunda camada de reranking, refinando as sugestões com base na similaridade das sinopses. O projeto culmina em uma avaliação rigorosa do desempenho do sistema, utilizando métricas padrão da indústria como Precisão e Recall.

## 2. Integrantes

* Anny
* Leticia
* Victor Freire

## 3. Arquitetura da Solução

Nosso sistema foi construído com uma arquitetura híbrida de duas etapas para maximizar a relevância e a qualidade das recomendações:

### Etapa 1: Motor de Recomendação (Filtragem Colaborativa)

-   **Técnica:** Utilizamos uma rede neural em Keras/TensorFlow para implementar a Fatoração de Matrizes, uma técnica avançada de Filtragem Colaborativa.
-   **Funcionamento:** O modelo aprende vetores de características latentes (***embeddings***) para cada usuário e cada filme. A afinidade entre um usuário e um filme é calculada através do produto escalar de seus embeddings, resultando em uma nota prevista. Com base nessas notas, uma lista inicial de recomendações Top-10 é gerada.

### Etapa 2: Camada de Reranking (Análise de Conteúdo com PLN)

-   **Técnica:** Empregamos um modelo de **Sentence-Transformers** (`all-MiniLM-L6-v2`) para vetorizar as sinopses dos filmes, capturando seu significado semântico.
-   **Funcionamento:** Para refinar a lista inicial, esta camada calcula um novo score para cada filme recomendado. O score é baseado na similaridade de cosseno de sua sinopse com a sinopse de um filme que o usuário **amou** (nota máxima) e um filme que o usuário **odiou** (nota mínima) em seu histórico. A lista é então reordenada com base neste novo score, e um Top-5 final é apresentado.

## 4. Tecnologias Utilizadas

-   **Linguagem:** Python 3
-   **Bibliotecas Principais:**
    -   **Manipulação de Dados:** `pandas`, `numpy`
    -   **Machine Learning / IA:** `scikit-learn`, `tensorflow.keras`
    -   **Processamento de Linguagem Natural:** `sentence-transformers`
    -   **Aquisição de Dados:** `requests` (para download do dataset e consumo da OMDb API)
    -   **Visualização e Utilitários:** `matplotlib`, `seaborn`, `tqdm`

## 5. Estrutura do Projeto

O notebook está organizado da seguinte forma:

1.  **Preparação do Ambiente:** Instalação e importação das dependências.
2.  **Coleta e Extração dos Dados:** Download e descompactação automática do dataset MovieLens 100k.
3.  **Carregamento e Limpeza:** Leitura dos dados para DataFrames e aplicação de tratamentos iniciais (remoção de colunas, padronização de títulos).
4.  **Engenharia de Features:** Transformação dos gêneros e agregação de métricas como nota média e contagem de avaliações.
5.  **Divisão dos Dados:** Separação dos dados em conjuntos de treino e teste com estratificação por usuário para uma avaliação rigorosa.
6.  **Modelagem e Treinamento:** Definição e treinamento do modelo de rede neural para previsão de notas.
7.  **Reranking com PLN:** Implementação da camada de refinamento com análise de sinopses.
8.  **Demonstração:** Geração de recomendações para um usuário de exemplo.
9.  **Avaliação Final:** Cálculo das métricas de Precisão@N e Recall@N para validar o desempenho do sistema.

## 6. Resultados

O sistema foi capaz de gerar recomendações personalizadas e coesas. A validação do modelo de previsão de notas e do sistema de recomendação Top-N apresentou os seguintes resultados:

### Desempenho do Modelo de Previsão de Notas
*(Resultados no conjunto de validação)*
-   **MSE (Mean Squared Error):** 0.8888
-   **MAE (Mean Absolute Error):** 0.7454
-   **RMSE (Root Mean Squared Error):** 0.9428

### Desempenho do Sistema de Recomendação Top-10
*(Métricas médias calculadas sobre o conjunto de teste)*
-   **Precisão Média @10:** 0.0970
-   **Recall Médio @10:** 0.0591

## 7. Como Executar
1.  **Ambiente:** Este notebook foi desenvolvido para ser executado no Google Colab.
2.  **Chave de API:** Para que a camada de reranking com sinopses funcione, é necessário inserir uma chave da OMDb API na célula correspondente.
3.  **Execução:** Basta executar as células em ordem sequencial. O processo de download dos dados, treinamento do modelo e geração de recomendações é totalmente automatizado.

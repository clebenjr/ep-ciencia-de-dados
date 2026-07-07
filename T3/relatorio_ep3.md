# Terceiro Exercício Programa — ACH2177 - Introdução à Ciência de Dados (2026)

**Cleben Junior Cosendey Garcia - 14691220**
**Daniel Coutinho Ribeiro - 13695063**
**Glauber Veloso Rocha - 13682997**
**João Pedro Oliveira de Paula Marcondes - 14582570**
**Luis Henrique Moraes - 14615590**

---

## 1. Introdução

Este trabalho apresenta a aplicação de modelos de aprendizado de máquina não supervisionado ao Student Performance Dataset, com o objetivo de identificar perfis de estudantes do ensino secundário com base em suas características socioeconômicas, familiares e comportamentais.

A questão de pesquisa, refinada ao longo dos trabalhos anteriores (EP1 e EP2), é: **a partir dos fatores socioeconômicos, familiares e comportamentais, que perfis de estudantes existem na base? Qual o desempenho acadêmico geral e nas áreas específicas de português e matemática destes perfis?**

Essa questão é relevante pois a identificação de perfis permite a formulação de políticas educacionais direcionadas, auxiliando na alocação de recursos e na detecção precoce de alunos em risco de reprovação.

A base de dados utilizada contém 370 registros de estudantes de duas escolas portuguesas, consolidados a partir da junção das bases de matemática e português. Foram aplicados os algoritmos K-Prototypes (principal) e K-Means (referência), conforme planejado no EP2. Os resultados indicam que os perfis encontrados refletem primariamente diferenças comportamentais — consumo de álcool, absenteísmo e nível de socialização — com fatores socioeconômicos e familiares contribuindo marginalmente para a diferenciação dos grupos.

---

## 2. Materiais e Métodos

### 2.1 Base de Dados

A base utilizada é o Student Performance Dataset (Cortez e Silva, 2008), descrevendo estudantes das escolas Gabriel Pereira e Mousinho da Silveira no ano letivo de 2005-2006. Após a curadoria descrita no EP2 (junção das bases, remoção de 12 registros inconsistentes e tratamento de dados faltantes), a base consolidada contém 370 registros com 39 variáveis.

As variáveis foram organizadas em 6 fatores, conforme definido no EP2:

| Fator | Variáveis | Papel |
|-------|-----------|-------|
| Socioeconômico | ESCOLARIDADE_MAE, ESCOLARIDADE_PAI, PROFISSAO_MAE, PROFISSAO_PAI | Entrada |
| Familiar | QUALIDADE_FAMILIA, SUPORTE_FAMILIAR, STATUS_PAIS, TAMANHO_FAMILIA | Entrada |
| Comportamental | TEMPO_ESTUDO_SEMANAL, ALCOOL_DIA_UTIL, ALCOOL_FIM_DE_SEMANA, SAIR_COM_AMIGOS, TEMPO_LIVRE, FALTAS_MAT, FALTAS_POR | Entrada |
| Recursos educacionais | SUPORTE_ESCOLAR, AULAS_PAGAS_MAT, AULAS_PAGAS_POR, INTERNET_CASA | Entrada |
| Motivação | ALMEJA_ENSINO_SUPERIOR, RAZAO_ESCOLHA_ESCOLA | Entrada |
| Desempenho | NOTA_3_MAT, NOTA_3_POR | Alvo (pós-clusterização) |

Total de variáveis de entrada: 21 (10 numéricas/ordinais + 11 categóricas/binárias).

### 2.2 Divisão dos Dados

A base foi dividida da seguinte forma (random_state=42 em todas as operações):

| Conjunto | Registros | Proporção | Uso |
|----------|-----------|-----------|-----|
| Treino | 259 | 70% | Treinamento dos modelos |
| Teste (hold-out) | 111 | 30% | Avaliação final |

### 2.3 Transformações nos Dados

- **Variáveis numéricas**: padronizadas com StandardScaler (fit no treino, transform em ambos). Necessário para que variáveis com escalas distintas (faltas 0-56 vs álcool 1-5) não dominem o cálculo de distância.
- **Variáveis categóricas (K-Prototypes)**: mantidas em formato original — o algoritmo utiliza a distância de correspondência (matching dissimilarity) para estas.
- **Variáveis categóricas (K-Means)**: codificadas via One-Hot Encoding (drop_first=True) e padronizadas junto às numéricas.
- **Variáveis-alvo (NOTA_3_MAT, NOTA_3_POR)**: excluídas do processo de clusterização e utilizadas apenas para caracterização posterior dos perfis.

### 2.4 Modelos Aplicados

**K-Prototypes** (algoritmo principal):
- Justificativa: lida nativamente com variáveis numéricas e categóricas simultaneamente, combinando K-Means (distância euclidiana para numéricas) com K-Modes (dissimilaridade de correspondência para categóricas).
- Implementação: biblioteca `kmodes` (Python), classe `KPrototypes`.
- Hiperparâmetros: init='Cao', n_init=5 (busca de K) / n_init=10 (modelo final), max_iter=100.

**K-Means** (modelo de referência):
- Justificativa: baseline amplamente utilizado; permite comparar o desempenho com e sem tratamento nativo de categóricas.
- Implementação: `sklearn.cluster.KMeans`.
- Hiperparâmetros: n_init=10, max_iter=300.

### 2.5 Avaliação

Para determinar o número ideal de clusters (K), foram testados valores de K=2 a K=8, avaliando:

- **Silhouette Score**: mede coesão intra-cluster e separação inter-cluster. Maior = melhor. Faixa: [-1, 1].
- **Davies-Bouldin Index**: razão entre dispersão intra-cluster e distância inter-cluster. Menor = melhor.
- **Calinski-Harabasz Index**: razão entre variância inter e intra-cluster. Maior = melhor.
- **Custo/Inércia**: métrica interna do algoritmo (método do cotovelo).

A generalização foi avaliada comparando as métricas obtidas no treino com as do conjunto de teste hold-out.

### 2.6 Ambiente Computacional

- Python 3.13.3
- Bibliotecas: pandas 3.0.2, numpy 2.4.4, scikit-learn 1.8.0, kmodes (K-Prototypes), matplotlib 3.10.9, seaborn 0.13.2
- Semente aleatória: random_state=42 em todas as operações estocásticas
- Código disponível em: https://github.com/clebenjr/ep-ciencia-de-dados

---

## 3. Resultados e Discussão

### 3.1 Seleção do K

**K-Prototypes:**

| K | Custo | Silhouette | Davies-Bouldin | Calinski-Harabasz |
|---|-------|-----------|----------------|-------------------|
| 2 | 2571.58 | **0.207** | 2.008 | **54.25** |
| 3 | 2325.38 | 0.131 | 1.954 | 46.77 |
| 4 | 2155.31 | 0.146 | 1.878 | 42.12 |
| 5 | 2042.00 | 0.126 | 1.882 | 37.58 |
| 6 | 1920.50 | 0.126 | 1.796 | 35.56 |
| 7 | 1860.78 | 0.110 | 1.894 | 32.39 |
| 8 | 1790.91 | 0.118 | **1.736** | 30.89 |

Silhouette e Calinski-Harabasz apontam para K=2. Davies-Bouldin sugere K=8, porém a melhoria é marginal e um número alto de clusters fragmentaria excessivamente a amostra (média de ~32 alunos por grupo).

**K-Means:**

| K | Inércia | Silhouette | Davies-Bouldin | Calinski-Harabasz |
|---|---------|-----------|----------------|-------------------|
| 2 | 6972.07 | 0.071 | 3.482 | 19.87 |
| 4 | 6296.57 | **0.100** | 2.884 | 16.39 |
| 6 | 5746.65 | 0.093 | **2.400** | 15.54 |

O K-Means apresentou silhouettes sistematicamente inferiores ao K-Prototypes (~0.07-0.10 vs ~0.13-0.21), confirmando que o tratamento nativo de variáveis categóricas é superior ao One-Hot Encoding para esta base.

**Decisão**: adotou-se K=2 com K-Prototypes como modelo principal.

[Inserir Figura: kprototypes_metricas.png]

[Inserir Figura: kmeans_metricas.png]

### 3.2 Caracterização dos Perfis

O modelo final (K-Prototypes, K=2) identificou dois perfis distintos:

**Perfil dos clusters (variáveis numéricas, médias):**

| Variável | Cluster 0 (n=79) | Cluster 1 (n=180) |
|----------|-------------------|---------------------|
| TEMPO_ESTUDO_SEMANAL | 1.63 | 2.18 |
| ALCOOL_DIA_UTIL | 2.32 | 1.13 |
| ALCOOL_FIM_DE_SEMANA | 3.67 | 1.74 |
| SAIR_COM_AMIGOS | 4.16 | 2.76 |
| TEMPO_LIVRE | 3.70 | 3.05 |
| FALTAS_MAT | 9.99 | 3.24 |
| FALTAS_POR | 6.54 | 2.44 |
| ESCOLARIDADE_MAE | 2.96 | 2.64 |
| ESCOLARIDADE_PAI | 2.66 | 2.48 |
| QUALIDADE_FAMILIA | 3.97 | 3.96 |

**Interpretação:**
- **Cluster 0 — "Perfil de Risco Comportamental"** (30.5% dos alunos): alunos com consumo de álcool significativamente mais alto (dia útil: 2.32 vs 1.13; fim de semana: 3.67 vs 1.74), maior socialização (4.16 vs 2.76), mais faltas (Mat: 10.0 vs 3.2; Por: 6.5 vs 2.4) e menor tempo de estudo (1.63 vs 2.18).
- **Cluster 1 — "Perfil Engajado"** (69.5% dos alunos): alunos com baixo consumo de álcool, menos faltas, maior dedicação ao estudo e menor frequência de saídas.

Nota-se que as variáveis categóricas (profissão dos pais, suporte familiar, status dos pais, tamanho da família, internet, almeja ensino superior, razão da escola) apresentaram modas **idênticas** entre os dois clusters. Isso indica que a diferenciação dos perfis é dominada pelo comportamento individual do aluno, e não por sua origem socioeconômica ou estrutura familiar.

### 3.3 Desempenho Acadêmico por Perfil

**Notas médias por cluster (treino):**

| Cluster | NOTA_3_MAT | NOTA_3_POR | NOTA_1_MAT | NOTA_1_POR |
|---------|-----------|-----------|-----------|-----------|
| 0 (Risco) | 9.30 | 10.95 | 9.66 | 10.87 |
| 1 (Engajado) | 10.88 | 13.24 | 11.33 | 12.69 |

O Cluster 0 apresenta média de Matemática abaixo do limiar de aprovação (10), enquanto o Cluster 1 mantém-se acima em ambas as disciplinas. A diferença é mais acentuada em Português (2.29 pontos) do que em Matemática (1.58 pontos).

[Inserir Figura: perfis_desempenho.png]

### 3.4 Validação no Conjunto de Teste

| Métrica | Treino | Teste |
|---------|--------|-------|
| Silhouette | 0.207 | 0.224 |
| Davies-Bouldin | 2.008 | 2.121 |
| Calinski-Harabasz | 54.25 | 19.06 |

A Silhouette manteve-se estável entre treino e teste (0.207 → 0.224), indicando que os clusters generalizam razoavelmente para dados não vistos. A queda no Calinski-Harabasz (54.25 → 19.06) é atribuída à diferença de tamanho amostral (259 vs 111 registros), dado que essa métrica é sensível ao número de observações.

A distribuição dos clusters no teste (29 vs 82) manteve proporção similar ao treino (79 vs 180 → ~30%/70%), reforçando a estabilidade dos perfis.

---

## 4. Limitações

1. **Silhouette baixo (0.207)**: indica que os clusters não são perfeitamente separáveis. Isso é esperado para dados socioeconômicos reais, mas implica que a fronteira entre os perfis é difusa — muitos alunos ficam em zonas intermediárias.

2. **Dominância comportamental**: os fatores Socioeconômico, Familiar, Recursos e Motivação não contribuíram efetivamente para a diferenciação dos clusters. Isso pode ser uma limitação do algoritmo (peso relativo numéricas vs categóricas) ou uma característica real dos dados — o comportamento individual é mais discriminante que a origem social nesta amostra.

3. **Variáveis com baixa variância**: ALMEJA_ENSINO_SUPERIOR (96.6% True) e as profissões (dominadas por "outro") não possuem poder discriminativo. Sua inclusão pode ter adicionado ruído ao modelo.

4. **Tamanho amostral limitado**: 259 registros de treino para 21 variáveis é uma razão baixa, o que restringe a complexidade dos modelos viáveis.

5. **Autodeclaração**: variáveis-chave para a separação (álcool, tempo de estudo) dependem da honestidade dos alunos, estando sujeitas a viés de desejabilidade social.

6. **Contexto específico**: os resultados referem-se a duas escolas portuguesas em 2005-2006, não sendo diretamente generalizáveis para outros contextos educacionais.

---

## 5. Conclusão

Este trabalho aplicou técnicas de clusterização (K-Prototypes e K-Means) ao Student Performance Dataset com o objetivo de identificar perfis de estudantes e avaliar seu desempenho acadêmico.

O modelo K-Prototypes com K=2 identificou dois perfis distintos: um de "risco comportamental" (30.5% dos alunos), caracterizado por alto consumo de álcool, elevado absenteísmo e baixa dedicação ao estudo; e um "engajado" (69.5%), com padrões opostos. O perfil de risco apresentou média de Matemática abaixo do limiar de aprovação, confirmando a associação entre comportamento e desempenho.

O K-Prototypes mostrou-se superior ao K-Means para esta base de dados mista, validando a escolha feita no planejamento (EP2). Os clusters demonstraram estabilidade satisfatória quando avaliados no conjunto de teste intocado.

Um achado inesperado foi a baixa contribuição dos fatores socioeconômicos e familiares para a diferenciação dos perfis, sugerindo que, nesta amostra, o comportamento individual do aluno é mais determinante para o agrupamento do que sua origem social.

### Direções Futuras

- **Testar K=3 ou K=4** para buscar subperfis mais descritivos (ex: separar alunos "engajados com suporte" de "engajados sem suporte").
- **Ajustar o parâmetro gamma** do K-Prototypes para modular o peso relativo das variáveis categóricas.
- **Remover variáveis de baixa variância** (ALMEJA_ENSINO_SUPERIOR, profissões) e reavaliar a qualidade dos clusters.
- **Aplicar modelos supervisionados** (Árvore de Decisão, Random Forest) para predizer risco de reprovação com base nos perfis identificados e na nota do primeiro período (G1).
- **Explorar técnicas de redução de dimensionalidade** (PCA, t-SNE) para visualização e interpretação dos clusters em espaço bidimensional.

---

## Referências

CORTEZ, P.; SILVA, A. Using data mining to predict secondary school student performance. In: FUTURE BUSINESS TECHNOLOGY CONFERENCE (FUBUTEC), 5., 2008, Porto. Proceedings [...]. Porto: Eurosis, 2008. p. 5-12.

CORTEZ, Paulo. Student Performance. UCI Machine Learning Repository, 2014. Disponível em: https://archive.ics.uci.edu/dataset/320/student+performance. Acesso em: 23 mar. 2026.

HUANG, Z. Extensions to the k-Means Algorithm for Clustering Large Data Sets with Categorical Values. Data Mining and Knowledge Discovery, v. 2, n. 3, p. 283-304, 1998.

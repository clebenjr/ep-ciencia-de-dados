# Fatores e Variáveis

Para responder à questão de pesquisa — a partir dos fatores socioeconômicos, familiares e comportamentais, que perfis de estudantes existem na base e qual o desempenho acadêmico desses perfis — as variáveis disponíveis no dataset foram organizadas em fatores. Cada fator representa uma dimensão conceitual que contribui para a formação dos perfis de alunos a serem identificados pela clusterização. O desempenho acadêmico, por sua vez, será utilizado como variável-alvo para caracterizar os perfis encontrados, não participando do processo de agrupamento.

Embora variáveis ordinais não possuam, rigorosamente, distâncias iguais entre categorias, optou-se por reportar média e desvio padrão como medidas complementares de tendência central e dispersão, prática comum na literatura educacional e em estudos com escalas Likert.

## Fator 1: Contexto Socioeconômico

Este fator busca capturar a condição socioeconômica da família do estudante. As variáveis que o compõem são: ESCOLARIDADE_MAE (ordinal, 0-4), ESCOLARIDADE_PAI (ordinal, 0-4), PROFISSAO_MAE (nominal) e PROFISSAO_PAI (nominal). O nível educacional e a ocupação dos pais são proxies clássicos de condição socioeconômica na literatura educacional, sendo amplamente utilizados em estudos sobre desempenho escolar.

Como limitação, destaca-se a ausência de uma variável direta de renda familiar no dataset. Famílias com baixa escolaridade podem ter boa condição financeira e vice-versa, de modo que o fator captura apenas parcialmente a dimensão socioeconômica.

A escolaridade das mães tende a ser superior à dos pais, conforme apresentado na Tabela 1: a moda é 4 (ensino superior) para as mães, contra 2 (5º ao 9º ano) para os pais. Isso sugere uma assimetria educacional entre os gêneros na geração dos pais desses alunos.

| Variável | Média | Mediana | Moda | Desvio Padrão |
|----------|-------|---------|------|---------------|
| ESCOLARIDADE_MAE | 2,70 | 3,0 | 4 | 1,09 |
| ESCOLARIDADE_PAI | 2,51 | 2,0 | 2 | 1,07 |

Essa diferença é visível na Figura 1, que apresenta a distribuição de ambas as variáveis.

[Inserir Figura 1: fator1_escolaridades.png — Distribuição da escolaridade da mãe e do pai]

Quanto às profissões, observa-se na Figura 2 o predomínio da categoria "outro" em ambos os casos, o que limita a granularidade da análise ocupacional. Nota-se também uma proporção significativamente maior de mães que trabalham em casa em relação aos pais.

[Inserir Figura 2: fator1_profissoes.png — Distribuição da profissão da mãe e do pai]

## Fator 2: Ambiente Familiar

Este fator visa representar a estrutura e a qualidade do suporte doméstico disponível ao estudante. As variáveis que o compõem são: QUALIDADE_FAMILIA (ordinal, 1-5), SUPORTE_FAMILIAR (binária), STATUS_PAIS (nominal) e TAMANHO_FAMILIA (nominal). A literatura reconhece o ambiente familiar como fator influente no desempenho escolar, tanto pelo suporte direto ao estudo quanto pela estabilidade emocional proporcionada.

Como limitação, a variável de qualidade familiar é autodeclarada em escala subjetiva (Likert 1-5), dependendo da percepção do próprio aluno.

A maioria dos alunos reporta boa qualidade familiar, com mediana 4 de 5, conforme a Tabela 2.

| Variável | Média | Mediana | Moda | Desvio Padrão |
|----------|-------|---------|------|---------------|
| QUALIDADE_FAMILIA | 3,94 | 4,0 | 4 | 0,92 |

As distribuições das demais variáveis deste fator são apresentadas na Figura 3. Observa-se que a maioria dos pais vive junto (88,9%), as famílias tendem a ter mais de 3 membros (71,5%) e cerca de 60% dos alunos recebem suporte familiar educacional.

[Inserir Figura 3: fator2_familiar.png — Distribuição das variáveis do ambiente familiar]

## Fator 3: Comportamento do Aluno

Este fator agrupa variáveis relativas aos hábitos e à rotina do estudante que podem impactar seu desempenho. As variáveis são: TEMPO_ESTUDO_SEMANAL (ordinal, 1-4), ALCOOL_DIA_UTIL (ordinal, 1-5), ALCOOL_FIM_DE_SEMANA (ordinal, 1-5), SAIR_COM_AMIGOS (ordinal, 1-5), TEMPO_LIVRE (ordinal, 1-5), FALTAS_MAT (discreta) e FALTAS_POR (discreta).

Como limitação, todas as variáveis ordinais são autodeclaradas, dependendo da sinceridade do aluno. Além disso, as faixas de tempo de estudo são desiguais (1=<2h, 2=2-5h, 3=5-10h, 4=>10h), mas serão tratadas como numéricas nos modelos, o que introduz uma aproximação.

O consumo de álcool é baixo em dias úteis (moda 1) mas aumenta nos fins de semana (média 2,41). A maioria dos alunos estuda entre 2 e 5 horas semanais. As faltas apresentam distribuição assimétrica à direita, com moda 0 mas valores extremos (até 56 em Matemática e 32 em Português). Esses valores são detalhados na Tabela 3.

| Variável | Média | Mediana | Moda | DP |
|----------|-------|---------|------|----|
| TEMPO_ESTUDO_SEMANAL | 1,99 | 2,0 | 2 | 0,86 |
| ALCOOL_DIA_UTIL | 1,52 | 1,0 | 1 | 0,90 |
| ALCOOL_FIM_DE_SEMANA | 2,41 | 2,0 | 1 | 1,30 |
| SAIR_COM_AMIGOS | 3,21 | 3,0 | 3 | 1,14 |
| TEMPO_LIVRE | 3,23 | 3,0 | 3 | 0,99 |
| FALTAS_MAT | 5,49 | 4,0 | 0 | 7,16 |
| FALTAS_POR | 3,93 | 2,0 | 0 | 5,11 |

As distribuições são apresentadas na Figura 4, onde a assimetria das faltas é particularmente evidente.

[Inserir Figura 4: fator3_comportamental.png — Distribuição das variáveis comportamentais]

## Fator 4: Recursos Educacionais

Este fator captura o acesso do estudante a oportunidades complementares de aprendizado. As variáveis são: SUPORTE_ESCOLAR (binária), AULAS_PAGAS_MAT (binária), AULAS_PAGAS_POR (binária) e INTERNET_CASA (binária).

Conforme a Figura 5, destaca-se a grande diferença entre aulas pagas de Matemática (48,8%) e Português (6,3%), sugerindo que Matemática é percebida como disciplina mais difícil pelos alunos e suas famílias. A maioria tem acesso à internet (84,1%), mas poucos recebem suporte escolar extra (13,0%).

[Inserir Figura 5: fator4_recursos.png — Distribuição das variáveis de recursos educacionais]

## Fator 5: Motivação e Aspiração

Este fator busca captar a intencionalidade e a ambição acadêmica do estudante. As variáveis são: ALMEJA_ENSINO_SUPERIOR (binária) e RAZAO_ESCOLHA_ESCOLA (nominal).

Como limitação, apenas duas variáveis compõem este fator, sendo insuficientes para captar a totalidade de um conceito multidimensional como motivação. Conforme a Figura 6, 96,6% dos alunos almejam ensino superior, o que reduz consideravelmente o poder discriminativo dessa variável para a clusterização. Quanto à razão de escolha da escola, a preferência pelo curso oferecido é a mais frequente (37,7%), seguida de proximidade de casa (28,0%) e reputação (26,1%).

[Inserir Figura 6: fator5_motivacao.png — Distribuição das variáveis de motivação]

## Fator 6: Desempenho Acadêmico (variável-alvo)

Este fator representa o resultado escolar do estudante e será utilizado para caracterizar os perfis encontrados pela clusterização, não participando do processo de agrupamento. As variáveis são: NOTA_3_MAT (discreta, 0-20) e NOTA_3_POR (discreta, 0-20). As notas dos períodos anteriores (G1 e G2) também estão disponíveis e poderão ser utilizadas em análises complementares, como a predição de risco de reprovação.

A escala de avaliação portuguesa classifica as notas da seguinte forma: abaixo de 10 como Insuficiente (reprovação), de 10 a 13 como Suficiente, de 14 a 15 como Bom, de 16 a 17 como Muito Bom, e de 18 a 20 como Excelente.

O desempenho em Português é consistentemente superior ao de Matemática (média 12,61 contra 10,35 na nota final). Matemática apresenta maior dispersão (desvio padrão de 4,55) e mais notas zero, indicando maior dificuldade ou taxa de abandono nessa disciplina. Os valores são detalhados na Tabela 4.

| Variável | Média | Mediana | Moda | DP | Mín | Máx |
|----------|-------|---------|------|----|-----|-----|
| NOTA_1_MAT | 10,77 | 10,0 | 10 | 3,32 | 3 | 19 |
| NOTA_2_MAT | 10,66 | 10,0 | 10 | 3,77 | 0 | 19 |
| NOTA_3_MAT | 10,35 | 11,0 | 10 | 4,55 | 0 | 20 |
| NOTA_1_POR | 12,17 | 12,0 | 12 | 2,38 | 7 | 19 |
| NOTA_2_POR | 12,29 | 12,0 | 13 | 2,36 | 7 | 19 |
| NOTA_3_POR | 12,61 | 13,0 | 13 | 2,72 | 0 | 19 |

As distribuições são apresentadas na Figura 7, onde se observa a diferença entre as disciplinas e a presença de notas zero.

[Inserir Figura 7: fator6_desempenho.png — Distribuição das notas por disciplina e período]

## Relação dos Fatores com a Questão de Pesquisa

Os fatores 1 a 5 (socioeconômico, familiar, comportamental, recursos educacionais e motivação) serão utilizados como entrada no processo de clusterização, com o objetivo de identificar perfis de estudantes com características semelhantes. O fator 6 (desempenho) será então cruzado com os perfis encontrados para responder à segunda parte da questão de pesquisa: qual o desempenho acadêmico de cada perfil identificado.

Essa separação entre variáveis de entrada e variável-alvo é fundamental para que os perfis reflitam genuinamente as condições de vida dos alunos, sem que o agrupamento seja enviesado pelo próprio resultado que se deseja explicar.

## Resumo do Mapeamento

| Fator | Variáveis | Tipo predominante | Papel na modelagem |
|-------|-----------|-------------------|-------------------|
| Socioeconômico | ESCOLARIDADE_MAE, ESCOLARIDADE_PAI, PROFISSAO_MAE, PROFISSAO_PAI | Ordinal + Nominal | Entrada |
| Familiar | QUALIDADE_FAMILIA, SUPORTE_FAMILIAR, STATUS_PAIS, TAMANHO_FAMILIA | Ordinal + Binária + Nominal | Entrada |
| Comportamental | TEMPO_ESTUDO_SEMANAL, ALCOOL_DIA_UTIL, ALCOOL_FIM_DE_SEMANA, SAIR_COM_AMIGOS, TEMPO_LIVRE, FALTAS_MAT, FALTAS_POR | Ordinal + Discreta | Entrada |
| Recursos educacionais | SUPORTE_ESCOLAR, AULAS_PAGAS_MAT, AULAS_PAGAS_POR, INTERNET_CASA | Binária | Entrada |
| Motivação | ALMEJA_ENSINO_SUPERIOR, RAZAO_ESCOLHA_ESCOLA | Binária + Nominal | Entrada |
| Desempenho | NOTA_3_MAT, NOTA_3_POR | Discreta | Alvo (pós-clusterização) |

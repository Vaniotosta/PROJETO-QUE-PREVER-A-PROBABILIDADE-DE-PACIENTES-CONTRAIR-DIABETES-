# PROJETO QUE PREVER A PROBABILIDADE DE PACIENTES CONTRAIR DIABETES 

# INTRODUÇÃO
Em todo o planeta, o diabetes afeta cerca de 537 milhões de pessoas. Para chamar à atenção de toda a população e de profissionais de saúde sobre a importância da prevenção, do diagnóstico precoce e do controle adequado da doença que este trabalho se faz importante haja visto que a diabetes pode ser encarado como questão de saúde pública. O diabetes atinge 10,2% da população brasileira, conforme dados da pesquisa Vigitel Brasil 2023 (Vigilância de Fatores de Risco e Proteção para Doenças Crônicas por Inquérito Telefônico). Índice representa aumento com relação a 2021, quando era 9,1%.Sendo assim, prevenir a doença através dos dados históricos de pacientes com análises de dados e modelagem preditiva se faz fundamental na luta e na redução de custos atrelado ao alto índices dos diagnósticos positivos.

## Desafio proposto:
Dado o aumento de casos de diabetes no Brasil, um plano de saúde deseja fornecer opções de medicina preventiva para seus clientes.

Utilizando o histórico médico de alguns pacientes eles gostariam de saber quais são os com a maior probabilidade de desenvolver diabetes.

A ideia é que os médicos desenvolvam planos de ação para esses pacientes, para que o hospital possa reduzir seus custos em longo prazo já que prevenir a doença é mais barato do que seu tratamento e suas complicações, além de melhorar o bem estar de seus clientes.

## Características da base

 - Cada linha são dados referentes a uma pessoa
 - Nós temos uma marcação das pessoas que são diabéticas
 - Alguns dos dados categóricos já foram agrupados previamente, por exemplo _Age_ e _PhysicallyActive_ já estão divididas em alguns grupos
 - Observando os nomes das colunas também temos indícios que os dados selecionados previamente parecem  ser conhecidos como influentes no desenvolvimento da doença (provavelmente nessa etapa teríamos que buscar a informações mais relevantes para o nosso problema em parceria com um médico por exemplo)

 - ## TREINAMENTO DE MODELOS

Uma boa estratégia a se seguir é a de testar modelos mais simples primeiro, e irmos aumentando a complexidade caso necessário.

Como citamos anteriormente, uma boa escolha neste caso poderia ser uma regressão logítica:

 - Ela é interpretável (isso sempre é bom, mesmo quando o problema não tem essa demanda)
 - É um modelo simples de explicar para os usuários
 - Retorna probabilidades (estava na descrição do problema)
 
Dessa forma vamos iniciar com ela
- penalty='none': O parâmetro penalty controla a regularização aplicada ao modelo de regressão logística. Quando definido como 'none', isso significa que nenhuma regularização está sendo aplicada. A regularização é usada para evitar overfitting, mas ao definir penalty como 'none', o modelo não está sujeito a nenhuma penalização nas ponderações, tornando-o uma regressão logística simples sem regularização.

- random_state=61658: O parâmetro random_state é usado para definir a semente aleatória, o que garante que os resultados sejam reproduzíveis. Ao definir um valor fixo, como 61658, o modelo gerará os mesmos resultados sempre que for ajustado aos mesmos dados. Isso é útil para fins de reprodutibilidade.

- max_iter=3_000: O parâmetro max_iter controla o número máximo de iterações que o otimizador utilizará para encontrar os parâmetros do modelo. Neste caso, o valor é definido como 3.000, o que significa que o otimizador tentará encontrar os parâmetros do modelo em até 3.000 iterações. Isso é importante para garantir que o otimizador tenha tempo suficiente para convergir para uma solução, especialmente se o problema for complexo.

  ## TENTATIVA DE MELHORAR O MODELO

  Neste caso, por sorte a curva roc já ficou muito boa com a abordagem escolhida e que o valor da métrica nos dados de teste está bastante próximo do observado no conjunto de treino (considendando média e os valores individuais dos folds)


Lembrando que essa abordagem envolve:

 - O classificador escolhido
 - Os parâmetros do classificador
 - Preenchimento de valores faltantes
 - Criação de variáveis (Feature Engineering)
 - Escolha do Scaler das variáveis
 - Encoding das variáveis categóricas

Todos os itens da lista acima podem ser modificados ou testados buscando uma melhor performance, mas sempre levando em conta __o tradeoff do tempo/esforço para realizar esses testes versus o ganho que pode trazer__ . 

Nesse caso, vamos tentar <u>reduzir nossa quantidade de variáveis</u> modificando o classificador para fazer de uma **regularização LASSO**

## Entendo as variáveis do modelo
O intercepto é -5,015. Isso significa que se todas as outras variáveis forem iguais a zero, então a probabilidade prevista de ter diabetes é -5,015. Este é um valor negativo, o que significa que o modelo prevê que é menos provável ter diabetes do que não ter diabetes.

O coeficiente de Pdiabetes_yes, 8.561, indica que um indivíduo com Pdiabetes_yes (pré-diabetes) tem uma probabilidade 8.561 vezes maior de ter diabetes do que um indivíduo com Pdiabetes_no (sem pré-diabetes).

O coeficiente de RegularMedicine_yes, 3.402, indica que um indivíduo que toma medicamentos regularmente tem uma probabilidade 3.402 vezes maior de ter diabetes do que um indivíduo que não toma medicamentos regularmente.

O coeficiente de BPLevel_low, -8.349, indica que um indivíduo com pressão arterial baixa tem uma probabilidade -8.349 vezes menor de ter diabetes do que um indivíduo com pressão arterial normal.

O coeficiente de Alcohol_yes, -0.525, indica que um indivíduo que ingere álcool tem uma probabilidade -0.525 vezes menor de ter diabetes do que um indivíduo que não ingere álcool.

O coeficiente para Idade_60 ou mais é 2,305. Isso significa que para cada aumento de uma unidade em Idade_60 ou mais, a probabilidade prevista de ter diabetes aumenta em 2,305. Este é um valor positivo, o que significa que o modelo prevê que as pessoas que têm 60 anos de idade ou mais são mais propensas a ter diabetes do que as pessoas mais jovens.

O coeficiente de Pregancies, 0.383, indica que um indivíduo com um ou mais gestações tem uma probabilidade 0.383 vezes maior de ter diabetes do que um indivíduo sem gestações. Em outras palavras, a probabilidade de um indivíduo com um ou mais gestações ter diabetes é de cerca de 38,3%.

Isso significa que a gravidez é um fator de risco para o diabetes. Estudos têm mostrado que o risco de desenvolver diabetes aumenta após a gravidez, mesmo em mulheres que não tinham diabetes antes da gravidez. Isso ocorre porque a gravidez pode causar alterações no corpo que podem levar à resistência à insulina e ao diabetes.

# Interpretando o modelo
De acordo com a informação fornecida, a probabilidade de um indivíduo pertencer à classe 1 (indivíduos com diabetes) é de 0,01% na ausência de qualquer outra variável. Isso significa que, para cada 100.000 indivíduos, apenas 1 indivíduo é esperado ter diabetes na ausência de qualquer outra variável.

Os padrões de indivíduos sem diabetes são aqueles que se afastam dos padrões que são associados ao diabetes. Esses padrões incluem:

Idade: Indivíduos mais jovens são menos propensos a ter diabetes do que indivíduos mais velhos.

Sexo: Indivíduos do sexo masculino são mais propensos a ter diabetes do que indivíduos do sexo feminino.

História familiar: Indivíduos com histórico familiar de diabetes são mais propensos a ter diabetes.

Fatores de risco comportamentais: Indivíduos com fatores de risco comportamentais, como obesidade, sedentarismo e má alimentação, são mais propensos a ter diabetes.

Portanto, indivíduos sem diabetes são mais propensos a:

Ser mais jovens.

Ser do sexo feminino.

Não ter histórico familiar de diabetes.

Não ter fatores de risco comportamentais.

Aqui estão alguns exemplos específicos de padrões de indivíduos sem diabetes:

Indivíduos com idade inferior a 40 anos são muito menos propensos a ter diabetes do que indivíduos com idade superior a 60 anos.
Indivíduos do sexo feminino são cerca de 20% menos propensos a ter diabetes do que indivíduos do sexo masculino.
Indivíduos sem histórico familiar de diabetes são cerca de 50% menos propensos a ter diabetes do que indivíduos com histórico familiar de diabetes.
Indivíduos com Índice de Massa Corporal (IMC) inferior a 25 são menos propensos a ter diabetes do que indivíduos com IMC superior a 30.
Indivíduos que praticam atividade física regularmente são menos propensos a ter diabetes do que indivíduos que são sedentários.
Indivíduos que seguem uma dieta saudável são menos propensos a ter diabetes do que indivíduos que seguem uma dieta não saudável.

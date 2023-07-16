---
layout: post
title: Kaagle - Modelo preditivo de turnover na empresa.
image: stephen-dawson-qwtCeJ5cLYs-unsplash.jpg
date: 2023-07-14 16:00:00 +0200
tags: modelo preditivo turnover machine learning random forest data science
categories: people_analytics
permalink: /category/people_analytics/kaagle-modelo-preditivo-de-turnover-na-empresa/
---
--
Problema de negócio - Construir uma máquina preditiva cujo objetivo é prever o turnover de funcionários da empresa
---

![It's Job Time!](https://media.giphy.com/media/btbUGSHh3f6eBjbDfh/giphy-downsized-large.gif)

# Análise exploratória de dados

A fim de iniciarmos nosso estudo de caso, será necessário instalar as bibliotecas Pandas e Numpy para tratarmos os dados na nossa base em CSV. Após isso seguiremos os seguintes passos:


1.   visualizar a nossa base de dados;
2.   Utilizando um describe, podemos tirar alguns insights interessantes.


# Dataset do Kaagle

![dataset - rh_analytics.csv](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover01.png)

------   

![Iniciando o projeto de análise exploratória](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover01.png)

------   

![Busca de dados](https://media.giphy.com/media/w3IE6RmM6bwrmLFEUb/giphy.gif)

# 3. Visualização de dados

Como nosso foco não é a visualização de dados em si, e sim a modelagem preditiva, para nos poupar tempo vamos utilizar a biblioteca DataPrep, uma ferramenta de visualização de dados que nos permite acelerar e simplificar o processo de preparação de dados em projetos de ciência de dados, permitindo uma análise mais eficiente e precisa, muito embora não precisemos utiliza-la em todos os projetos.

Além disso, a biblioteca DataPrep fornece uma interface simples e intuitiva, que facilita a manipulação e visualização dos dados. Isso torna mais fácil identificar padrões, detectar anomalias e entender a estrutura dos dados antes de prosseguir com as etapas de modelagem e análise.

Neste caso, vamos observar a distribuição para compreender como o colaborador se comporta de acordo com cada variável de nossa base.

![Importando DataPrep](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover2.png)

![DataPrep-overview](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover3.png)

![DataPrep-Variables1](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover4.png)

![DataPrep-Variables2](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover5.png)

![DataPrep-Variables3](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover6.png)

![DataPrep-Variables3](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover7.png)

![DataPrep-Variables5](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover8.png)

![DataPrep-interactions](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover9.png)

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover10.png)




![Tratamento de dados](https://media.giphy.com/media/Jn4GqDQRFM1oVV2WCP/giphy-downsized-large.gif)
# 3. Tratando o dataset

Ao observar os dados notamos que podemos simplificar nosso trabalho renomeando alguns campos. Por exemplo:
O campo 'Sales', que na verdade, deveria chamar-se 'department'.

Outra alteração relevante será unificar departamentos correlatos, que em sua nomenclatura, pouco explicam nossos dados. Feito isso, temos a quantidade de funcionários por departamento, utilizando a função 'hr['department'].values_counts()'.

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover11.png)

A seguir, utilizamos o One-Hot Encoding, uma técnica de transformação de variáveis categóricas em formato numérico para uso em algoritmos de aprendizado de máquina. Ele cria novas colunas binárias para cada categoria única da variável, atribuindo o valor 1 para a categoria presente e 0 para as demais. Isso permite que os modelos interpretem as informações categóricas de forma adequada, melhorando a precisão e desempenho da modelagem de dados.

O próximo passo é eliminar nossa variável target, que afinal trata-se da variável 'left', a fim de que nosso modelo descubra por si só qual é o resultado desta coluna. Ou seja, nosso colaborador deixará ou não deixará a empresa.

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover12.png)

------
![Modelagem de dados](https://media.giphy.com/media/tJFkuce3fKf4Aw6jRH/giphy.gif)


# 4. Vamos à modelagem!

Uma vez importadas as biblitecas scikit-learn e de Machine learning, vamos iniciar a modelagem.

Trabalharemos com 3 modelos de classificação. Sendo eles, Regressão Logística, Random Forest e XG Boost.

De forma bem resumida, a **Regressão Logística** é um modelo de aprendizado de máquina que é usado principalmente para problemas de classificação binária, onde o objetivo é prever a probabilidade de uma observação pertencer a uma determinada classe. Ela se baseia em uma função logística para modelar a relação entre as variáveis independentes e a variável dependente.

-

Por outro lado, o **Random Forest** é um algoritmo de aprendizado de máquina que utiliza um conjunto de árvores de decisão para realizar tarefas de classificação ou regressão. Cada árvore é construída de forma independente e a previsão final é feita através de uma votação ou média das previsões individuais das árvores.

-

E por último temos, o **XGBoost** (Extreme Gradient Boosting) é um algoritmo baseado em árvores de decisão que utiliza o método de boosting para construir um modelo preditivo. Ele treina um conjunto de árvores de forma sequencial, onde cada nova árvore é construída para corrigir os erros do modelo anterior.

-

Em resumo, a **Regressão Logística** é um modelo linear usado para classificação, enquanto o Random Forest e o XGBoost são algoritmos baseados em árvores de decisão, com o XGBoost utilizando a técnica de boosting para melhorar o desempenho.

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover15.png)

Neste momento separamos as variáveis X e Y. Sendo que Y("Left") é a variável que eu quero descobrir.

Rodamos o primeiro modelo, de Regressão logística e chegamos a uma acurácia de 77%. Um resultado ok.

**Será que podemos melhorar esse resultado com outras opções de algorítmos?**

Vamos tentar o Random Forest:

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover16.png)

Ótimo, conseguimos melhorar nosso Score. 97.6% de Acurácia.



![Yes!](https://media.giphy.com/media/3o6UB3VhArvomJHtdK/giphy.gif)


**E se aplicarmos outro modelo preditivo, será que o Random Forest realmente é o mais adequado para esta base de dados?**


![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover17.png)

Após aplicarmos o XG Bosst percebemos que de fato, neste caso, vamos considerar que o Random Forest atingiu melhor acurácia diante dos métodos de avaliação.

Para termos certeza de que estamos optando mesmo pelo modelo mais eficaz, utilizamos algumas métricas.

A primeira é a **Classification Report**, métrica de avaliação de desempenho de modelos de classificação que fornece uma visão geral das principais indicadores, como **precisão**, **recall** e **f1-score**, para cada classe do problema. Ele também inclui a média ponderada dessas métricas para avaliar o desempenho geral do modelo.


![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover25.png)

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover18.png)

A segunta utilizada é a **Confusion Matrix**, que nos mostra a contagem de verdadeiros positivos, verdadeiros negativos, falsos positivos e falsos negativos do modelo.
-
Ela é útil para entender como o modelo está classificando corretamente e incorretamente as amostras em cada classe, fornecendo uma visão mais detalhada do desempenho do modelo.

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover19.png)

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover24.png)


E por último, a **Curva ROC (Receiver Operating Characteristic)** uma representação gráfica da performance de um modelo de classificação binária variando o threshold de classificação. Ela traça a taxa de verdadeiros positivos (TPR) versus a taxa de falsos positivos (FPR). A área sob a curva (AUC-ROC) é uma medida da qualidade geral do modelo, onde um valor próximo de '1' indica um modelo com boa capacidade de distinguir entre classes.

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover21.png)

Essas métricas e visualizações são amplamente utilizadas na avaliação de modelos de classificação, fornecendo insights sobre seu desempenho, capacidade de generalização e capacidade de discriminar entre as classes.

------

# 5. Quais as variáveis mais relevantes? 

Isto posto, utilizamos o próprio Random Forest para entendermos quais variáveis são mais importantes nessa previsão. 

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover22.png)

# 6. E agora, vamos testar o modelo? 

Utilizamos a parte da nossa base que não foi utilizada para treino, e sem a variável Y. Observe que o modelo de fato conseguiu prever a coluna 'Left' conforme desejávamos. 

![DataPrep-correlations](https://raw.githubusercontent.com/alesouzaeu/alesouzaeu.github.io/master/images/previsão-turnover23.png)





------   



![Valorização do empregado](https://media.giphy.com/media/IhWIPeLT9UtAG1Uf4U/giphy.gif)

# 7. Conclusão

A predição de turnover por meio de machine learning pode trazer vários benefícios para uma empresa. Ao identificar antecipadamente os funcionários propensos a deixar a organização, a empresa pode tomar medidas proativas para reter esses talentos valiosos. 
-
Isso pode incluir a implementação de políticas de retenção, programas de desenvolvimento de carreira, melhorias nas condições de trabalho ou até mesmo a oferta de incentivos personalizados. 
-
Essas ações podem ajudar a reduzir a rotatividade de funcionários, economizando recursos financeiros e tempo de treinamento investidos em novas contratações. Além disso, a predição de turnover permite que a empresa identifique os fatores de risco e as tendências subjacentes que levam ao afastamento dos funcionários.Com essas informações, é possível realizar análises mais aprofundadas sobre o ambiente de trabalho, políticas de gestão e satisfação dos funcionários, visando a implementação de melhorias específicas e estratégias de engajamento. Isso resulta em um ambiente de trabalho mais saudável, aumento da produtividade, melhor colaboração e um clima organizacional positivo. 
-
De forma geral, a predição de turnover por meio de machine learning oferece grande vantagem competitiva significativa, permitindo que a empresa adote uma abordagem proativa para a gestão de talentos, melhore a retenção de funcionários e crie um ambiente de trabalho mais saudável e produtivo.


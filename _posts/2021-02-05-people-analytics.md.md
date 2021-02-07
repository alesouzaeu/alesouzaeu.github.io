---
layout: post
title: Análise de turnover
image: 14.jpg
date: 2021-02-05 13:35:20 +0200
tags:
categories: PeopleAnalytics
---

# Avaliando o perfil de colaboradores que se desligaram da empresa

## Objetivo

O  foco  inicial do projeto é identificar os perfis de colaboradores que tendem a pedir demissão nessa empresa, de acordo com a base de dados fornecida pela IBM.
  Esta  base foi  obtida  no site do **Kaggle**

  Para esse caso vamos aplicar as ferramentas: **Matrix Correlation e Biblioteca Seaborn**


```python
#carregando as bibliotecas necessárias
%matplotlib inline
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

#Tirando Warnings do código
import warnings
warnings.filterwarnings('ignore')
```


```python
df = pd.read_csv('HR_comma_sep.csv', dtype={'left':'category'})
# Ao importar o dataset, já indicamos que nossa variável de saída é categórica.
```


```python
# Renomeando variáveis.
df.rename(columns={"satisfaction_level":"nivel_satisfacao","last_evaluation":"ultima_avaliacao", "number_project":"numero_projetos", "average_montly_hours":"media_horas_mensal", "time_spend_company":"tempo_empresa", "Work_accident":"sofreu_acidente", "promotion_last_5years":"promocao_ultimos_5anos", "left":"demissao", "sales":"depto","salary":"salario"}, inplace=True)
```

### verificando se existe algum dano Null em nossa base de dados. 

```python

df.isnull().any()
```




    nivel_satisfacao          False
    ultima_avaliacao          False
    numero_projetos           False
    media_horas_mensal        False
    tempo_empresa             False
    sofreu_acidente           False
    demissao                  False
    promocao_ultimos_5anos    False
    depto                     False
    salario                   False
    dtype: bool



### Entendendo o Dataset


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nivel_satisfacao</th>
      <th>ultima_avaliacao</th>
      <th>numero_projetos</th>
      <th>media_horas_mensal</th>
      <th>tempo_empresa</th>
      <th>sofreu_acidente</th>
      <th>demissao</th>
      <th>promocao_ultimos_5anos</th>
      <th>depto</th>
      <th>salario</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.38</td>
      <td>0.53</td>
      <td>2</td>
      <td>157</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>sales</td>
      <td>low</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.80</td>
      <td>0.86</td>
      <td>5</td>
      <td>262</td>
      <td>6</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>sales</td>
      <td>medium</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.11</td>
      <td>0.88</td>
      <td>7</td>
      <td>272</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>sales</td>
      <td>medium</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.72</td>
      <td>0.87</td>
      <td>5</td>
      <td>223</td>
      <td>5</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>sales</td>
      <td>low</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.37</td>
      <td>0.52</td>
      <td>2</td>
      <td>159</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>sales</td>
      <td>low</td>
    </tr>
  </tbody>
</table>
</div>



### Quantos colaboradores temos para cada departamento? 

![png](https://media.giphy.com/media/MsWnkCVSXz73i/giphy.gif)

```python
df['depto'].value_counts()
```




    sales          4140
    technical      2720
    support        2229
    IT             1227
    product_mng     902
    marketing       858
    RandD           787
    accounting      767
    hr              739
    management      630
    Name: depto, dtype: int64




```python
#vamos converter departamento e média salarial para variável numérica (Algumas funções não funcionam com strings)
df['depto'].replace(['sales','technical','support','IT','product_mng','marketing','RandD','accounting','hr','management'],[0,1,2,3,4,5,6,7,8,9], inplace = True)
df['salario'].replace(['low', 'medium', 'high'], [0,1,2], inplace=True)
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nivel_satisfacao</th>
      <th>ultima_avaliacao</th>
      <th>numero_projetos</th>
      <th>media_horas_mensal</th>
      <th>tempo_empresa</th>
      <th>sofreu_acidente</th>
      <th>demissao</th>
      <th>promocao_ultimos_5anos</th>
      <th>depto</th>
      <th>salario</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.38</td>
      <td>0.53</td>
      <td>2</td>
      <td>157</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.80</td>
      <td>0.86</td>
      <td>5</td>
      <td>262</td>
      <td>6</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.11</td>
      <td>0.88</td>
      <td>7</td>
      <td>272</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.72</td>
      <td>0.87</td>
      <td>5</td>
      <td>223</td>
      <td>5</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.37</td>
      <td>0.52</td>
      <td>2</td>
      <td>159</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#vamos colocar o campo demissao em primeiro lugar, pois ele é muito importante

front = df['demissao']
df.drop(labels=['demissao'], axis=1, inplace=True)
df.insert(0, 'demissao', front)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>demissao</th>
      <th>nivel_satisfacao</th>
      <th>ultima_avaliacao</th>
      <th>numero_projetos</th>
      <th>media_horas_mensal</th>
      <th>tempo_empresa</th>
      <th>sofreu_acidente</th>
      <th>promocao_ultimos_5anos</th>
      <th>depto</th>
      <th>salario</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.38</td>
      <td>0.53</td>
      <td>2</td>
      <td>157</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.80</td>
      <td>0.86</td>
      <td>5</td>
      <td>262</td>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>0.11</td>
      <td>0.88</td>
      <td>7</td>
      <td>272</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0.72</td>
      <td>0.87</td>
      <td>5</td>
      <td>223</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0.37</td>
      <td>0.52</td>
      <td>2</td>
      <td>159</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (14999, 10)




```python
df.dtypes
```




    demissao                  category
    nivel_satisfacao           float64
    ultima_avaliacao           float64
    numero_projetos              int64
    media_horas_mensal           int64
    tempo_empresa                int64
    sofreu_acidente              int64
    promocao_ultimos_5anos       int64
    depto                        int64
    salario                      int64
    dtype: object



### Qual será a taxa de pedidos de demissão da empresa?


```python
taxa_abandono = df.demissao.value_counts() /14999
taxa_abandono
```




    0    0.761917
    1    0.238083
    Name: demissao, dtype: float64



### Podemos entender que a Taxa de abandono é de 23% 

### Overview baseado na coluna demissão


```python
# Um Overview baseado na coluna demissão (Abandono pelas demais variáveis)

abandono_overview = df.groupby('demissao')
abandono_overview.mean()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nivel_satisfacao</th>
      <th>ultima_avaliacao</th>
      <th>numero_projetos</th>
      <th>media_horas_mensal</th>
      <th>tempo_empresa</th>
      <th>sofreu_acidente</th>
      <th>promocao_ultimos_5anos</th>
      <th>depto</th>
      <th>salario</th>
    </tr>
    <tr>
      <th>demissao</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.666810</td>
      <td>0.715473</td>
      <td>3.786664</td>
      <td>199.060203</td>
      <td>3.380032</td>
      <td>0.175009</td>
      <td>0.026251</td>
      <td>2.739237</td>
      <td>0.650945</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.440098</td>
      <td>0.718113</td>
      <td>3.855503</td>
      <td>207.419210</td>
      <td>3.876505</td>
      <td>0.047326</td>
      <td>0.005321</td>
      <td>2.555587</td>
      <td>0.414730</td>
    </tr>
  </tbody>
</table>
</div>



### Insights

    1- Podemos notar que pessoas que pedem demissão ou abandonam a empresa estão 22% menos satisfeitas
    2- As pessoas que pedem demissão trabalham em média de 8 a 10 horas dia / mês
    3- O número de projetos em que a pessoa está envolvida ou o tempo de empresa parecem não influenciar na decisão de abandono
    4- As pessoas que abandonam a empresa que tem uma taxa de promoção MUITO menor que aqueles que permanecem na empresa.
    5- O salário médio de quem abandona a empresa é muito menor.

#### Uma visão mais estatistica do dataset


![png](https://media.giphy.com/media/ne3xrYlWtQFtC/giphy.gif)

```python


df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1"  class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nivel_satisfacao</th>
      <th>ultima_avaliacao</th>
      <th>numero_projetos</th>
      <th>media_horas_mensal</th>
      <th>tempo_empresa</th>
      <th>sofreu_acidente</th>
      <th>promocao_ultimos_5anos</th>
      <th>depto</th>
      <th>salario</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
      <td>14999.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.612834</td>
      <td>0.716102</td>
      <td>3.803054</td>
      <td>201.050337</td>
      <td>3.498233</td>
      <td>0.144610</td>
      <td>0.021268</td>
      <td>2.695513</td>
      <td>0.594706</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.248631</td>
      <td>0.171169</td>
      <td>1.232592</td>
      <td>49.943099</td>
      <td>1.460136</td>
      <td>0.351719</td>
      <td>0.144281</td>
      <td>2.754845</td>
      <td>0.637183</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.090000</td>
      <td>0.360000</td>
      <td>2.000000</td>
      <td>96.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.440000</td>
      <td>0.560000</td>
      <td>3.000000</td>
      <td>156.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.640000</td>
      <td>0.720000</td>
      <td>4.000000</td>
      <td>200.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.820000</td>
      <td>0.870000</td>
      <td>5.000000</td>
      <td>245.000000</td>
      <td>4.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>5.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>7.000000</td>
      <td>310.000000</td>
      <td>10.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>9.000000</td>
      <td>2.000000</td>
    </tr>
  </tbody>
</table>
</div>



### Será que existe correlação entre as variáveis? Vamos analizar a Correlation Matrix.


```python
plt.figure(figsize= (16,8))

corr = df.corr()
corr = (corr)
sns.heatmap(corr,
            xticklabels=corr.columns.values,
            yticklabels=corr.columns.values,
           annot=True,
           cmap="YlGnBu", linewidths=0)
corr
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nivel_satisfacao</th>
      <th>ultima_avaliacao</th>
      <th>numero_projetos</th>
      <th>media_horas_mensal</th>
      <th>tempo_empresa</th>
      <th>sofreu_acidente</th>
      <th>promocao_ultimos_5anos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>nivel_satisfacao</th>
      <td>1.000000</td>
      <td>0.105021</td>
      <td>-0.142970</td>
      <td>-0.020048</td>
      <td>-0.100866</td>
      <td>0.058697</td>
      <td>0.025605</td>
    </tr>
    <tr>
      <th>ultima_avaliacao</th>
      <td>0.105021</td>
      <td>1.000000</td>
      <td>0.349333</td>
      <td>0.339742</td>
      <td>0.131591</td>
      <td>-0.007104</td>
      <td>-0.008684</td>
    </tr>
    <tr>
      <th>numero_projetos</th>
      <td>-0.142970</td>
      <td>0.349333</td>
      <td>1.000000</td>
      <td>0.417211</td>
      <td>0.196786</td>
      <td>-0.004741</td>
      <td>-0.006064</td>
    </tr>
    <tr>
      <th>media_horas_mensal</th>
      <td>-0.020048</td>
      <td>0.339742</td>
      <td>0.417211</td>
      <td>1.000000</td>
      <td>0.127755</td>
      <td>-0.010143</td>
      <td>-0.003544</td>
    </tr>
    <tr>
      <th>tempo_empresa</th>
      <td>-0.100866</td>
      <td>0.131591</td>
      <td>0.196786</td>
      <td>0.127755</td>
      <td>1.000000</td>
      <td>0.002120</td>
      <td>0.067433</td>
    </tr>
    <tr>
      <th>sofreu_acidente</th>
      <td>0.058697</td>
      <td>-0.007104</td>
      <td>-0.004741</td>
      <td>-0.010143</td>
      <td>0.002120</td>
      <td>1.000000</td>
      <td>0.039245</td>
    </tr>
    <tr>
      <th>promocao_ultimos_5anos</th>
      <td>0.025605</td>
      <td>-0.008684</td>
      <td>-0.006064</td>
      <td>-0.003544</td>
      <td>0.067433</td>
      <td>0.039245</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




![png](/images/p-analytics_23_1.png)


### Insights

        1- Podemos dizer que existem correlações evidentes
        2-  (+) numero_projetos, numero de horas trabalhadas e a última avaliação
        3-  (-) volume de projetos, satisfação e salário

- Pode significar que os funcionários que passam mais horas e fazem mais projetos foram altamente avaliados.
- Mas a avaliação de recursos, quando comparada de forma independente com a rotatividade variável de resposta, mostra pouca ou nenhuma relação.

- O que significa? Para os relacionamentos negativos (-), o abandono, a satisfação e o salário são altamente correlacionados.
- As pessoas que tendem a deixar a empresa, mais quando estão menos satisfeitas e mal remuneradas.

### Departamento vs Abandono



```python

depto_abandono = pd.crosstab(index=df['depto'], 
                            columns=df['demissao'])
depto_abandono.plot(kind='bar',
                   figsize=(20,10),
                   stacked=True)


```




    <AxesSubplot:xlabel='depto'>




![png](/images/p-analytics_25_1.png)


![png](https://media.giphy.com/media/YlAQJmceNSN2dlsyQc/giphy.gif)

### Departamento vs Média Salarial




```python

depto_salario = pd.crosstab(index=df['depto'],
                            columns=df['salario'])
depto_salario.plot(kind="bar", figsize=(20,10), stacked=True)
```




    <AxesSubplot:xlabel='depto'>




![png](/images/p-analytics_26_1.png)

![png](https://media.giphy.com/media/5fBH6z8aMV1RbA4FaSc/giphy.gif)

### Perspectiva de crescimento na área de vendas é menor. Muitos colaboradores para poucas posições de liderança.


# Média Salarial vs Abandono

```python

salario_abandono = pd.crosstab(index=df['salario'],
                            columns=df['demissao'])
salario_abandono.plot(kind="bar", figsize=(20,10), stacked=True)
```




    <AxesSubplot:xlabel='salario'>




![png](/images/p-analytics_28_1.png)



### Promoção nos últimos 5 anos vs Abandono

```python

ult_promocao_demissao = pd.crosstab(index=df['promocao_ultimos_5anos'],
                            columns=df['demissao'])
ult_promocao_demissao.plot(kind="bar", figsize=(20,10), stacked=True)
```




    <AxesSubplot:xlabel='promocao_ultimos_5anos'>




![png](/images/p-analytics_29_1.png)


### Ao que parece a maioria ou quase todos os empregados que saíram, não foram promovidos nos últimos 5 anos.

![png](https://media.giphy.com/media/kBMQvQ27ax17nGj1v6/giphy.gif)

### Tempo de empresa vs Abandono

```python

tempo_empresa_demissao = pd.crosstab(index=df['tempo_empresa'],
                            columns=df['demissao'])
tempo_empresa_demissao.plot(kind="barh", figsize=(20,10), stacked=True)
```




    <AxesSubplot:ylabel='tempo_empresa'>




![png](/images/p-analytics_31_1.png)


### Insights
    1- A zona de perigo de abandono se dá entre 3 e 6 anos de empresa. (Nesta faixa, por que não apresentar propostas para motivar este grupo)
    2- Note que ninguém que tem de 7 a 10 anos de empresa, saiu da empresa.


![png](https://media.giphy.com/media/XCxcmEQWxDdc8qsd2R/giphy.gif)



### Numero de Projetos vs Abandono

```python

num_projetos_demissao = pd.crosstab(index=df['numero_projetos'],
                            columns=df['demissao'])
num_projetos_demissao.plot(kind="bar", figsize=(20,10), stacked=True)
```




    <AxesSubplot:xlabel='numero_projetos'>




![png](/images/p-analytics_33_1.png)


### Insights    

    1- Pessoas envolvidas em 2 projetos, mais da metade pedem demissão. Será que elas se sentem desvalorizadas por trabalharem pouco?
    2- Praticamente todas as pessoas que trabalhavam em 7 projetos pediram demissão. (Quem é que aguenta 7 projetos sem estafar?)
    3- A partir de 4 projetos, o número de abandono/demissão aumenta conforme aumentam proporcionalmente à quantidade de projetos.
    4- Neste caso fica claro que 3 projetos é a quantidade ideal para reter um colaborador.

### Vamos verificar a relação entre ultima avaliação e pedido de demissão

Gráfico de densidade.


```python
fig2 = plt.figure(figsize=(20,6),)
sns.kdeplot(
   data=df, x="ultima_avaliacao", hue="demissao",
   fill=True, common_norm=False, palette="tab10",
   alpha=.7, linewidth=2,
)
plt.title('Última avaliação')

```




    Text(0.5, 1.0, 'Última avaliação')




![png](/images/p-analytics_36_1.png)


### Vamos verificar a relação entre as horas e o abandono

Gráfico de densidade.


```python
# Vamos motrar um gráfico de densidade.

fig2 = plt.figure(figsize=(20,6),)
sns.kdeplot(
   data=df, x="media_horas_mensal", hue="demissao",
   fill=True, common_norm=False, palette="tab10",
   alpha=.7, linewidth=2,
)
plt.title('Media de horas mensais')

```




    Text(0.5, 1.0, 'Media de horas mensais')




![png](/images/p-analytics_38_1.png)


### Insights

    1- Notamos outro bi-modal!
    2- Quem trabalhou de menos saiu juntamente com quem trabalhou de mais.

### Vamos verificar a relação entre a demissão e média de projetos

![png](https://media.giphy.com/media/3oriO0DjQt8QCUUDL2/giphy.gif)

```python
fig2 = plt.figure(figsize=(20,6),)
sns.kdeplot(
   data=df, x="nivel_satisfacao", hue="demissao",
   fill=True, common_norm=False, palette="tab10",
   alpha=.7, linewidth=2,
)
plt.title('Distribuição do Nível de satisfação por demissão')

```




    Text(0.5, 1.0, 'Distribuição do Nível de satisfação por demissão')




![png](/images/p-analytics_41_1.png)


### Quais são os grupos de colaboradores da minha base? 


```python
sns.lmplot(x='nivel_satisfacao', y='ultima_avaliacao', data=df,
          fit_reg=True, #Configurando linha de regressão
          hue='demissao',
            palette="tab10",
           height=10)
```




    <seaborn.axisgrid.FacetGrid at 0x165ceb34488>




![png](/images/p-analytics_43_1.png)




#### São três personas que estão saindo da minha empresa.

        1- Pessoas que são muito boas, que estão extremamente insatisfeitas.
        2- Pessoas que são muito boas, que estão extremamente satisfeitas.
        3- Pessoas que são medianas, que estão mais ou menos satisfeitas.

#### A nível de RH algumas ações podem ser tomadas com base neste ensaio de People Analytics.

Partindo do princípio que de 3 e 6 anos de empresa, a evasão é considerável, olharemos para esta faixa com mais carinho.

Entendemos que o ponto mais crítico é com relação aos *colaboradores extremamente bons, que estão extremamente insatisfeitos.* Não queremos perdê-los não é mesmo? 

### O que faremos?

1 - Em especial notamos durante o estudo que os colaboradores que mais trabalham são vistos pelos gestores como os mais bem avaliados. Com base na somatória de horas mensal de cada um, a proposta é reavaliar a quantidade de projetos proposta por seus gestores. O objetivo é não sobrecarregá-los, para que tenham tempo para curtir suas famílias, estudar, ter momentos de lazer e diminuir assim o extress. O total de três projetos seria a nossa meta para estes ótimos colaboradores insatisfeitos, que certamente costumam trabalhar até estafar.

2 - Ainda será necessário rever o plano de carreira para o departamento de vendas. Tratam-se de muitos colaboradores em um setor que não oferece muitas oportunidades de crescimento. O que faz com que os excelentes funcionários dali, desejem sair ao mercado à procura da promoção que não conseguiram nos últimos 5 anos. Se não for possível subí-los de cargo, pode-se avaliar (os pontos positivos e negativos diante da cultura atual) a implantação de um projeto de incentivo à carreira em *W*. Ótima possibilidade para valorizar, reter, promover o pensamento sistêmico e a criação de novas equipes multifuncionais.

3 - Ao grupo dos *colaboradores muito insatisfeitos e mal avaliados por seus gestores*, deve-se abrir espaço para uma proposta de desenvolvimento que permita identificar potenciais talentos que tem dificuldades de se desenvolverem na estrutura atual. Novamente assumimos que os gestores desta empresa, tendem a valorizar os que mais trabalham, com projetos e que fazem mais horas, este grupo de funcionários pode estar sentindo pouco aproveitados e até mesmo acomodados com seus 2 projetos. É preciso tirá-los da zona de conforto, capacitá-los e aproveitá-los. 

        
### Conclusão

Note segmentar os grupos do seu quadro de funcionários de acordo com os critérios da sua base de dados poderá auxiliá-lo a não desperdiçar dinheiro investindo em planos de desenvolvimentos genéricos e pouco eficazes. Pense no quanto você pode otimizar o crescimento dos talentos de sua companhia olhando de forma individual para aquele colaborador que realmente precisa da sua atenção.
---
layout: post
title: Análise de audiencia das novelas na Rede Globo
image: 13.jpg
date: 2021-02-06 13:35:20 +0200
tags:
categories: PeopleAnalytics
---

**Sobre este projeto**

Esta é a minha primeira Análise Exploratória de Dados, fazendo uso da biblioteca **Plotly**. Para desenvolver este estudo, utilizei uma base de dados fornecida por Rubens Alves, no Kaggle, onde são apresentadas algumas informações sobre a audiência das novelas da Rede Globo. Como a comunicação me fascina, resolvi partilhar insights, tomando em conta as novelas, seus autores e a quantidade de pontos de audiência, aferidos entre 1966 e 2019. Espero que goste!

dataset: https://www.kaggle.com/rubaoalves/novelas-globo

Disclaimer: Antes que você se pergunte, não sou fã de novelas e nem as assisto. No entanto tudo o que envolve comunicação me instiga. 

**Então, vamos lá!** 


**O Plotly**

O Plotly é uma biblioteca de visualização maravilhosa que nos permite além de visualizar, interagir com os gráficos plotados em em nossa análise de dados.

Documentação: https://plotly.com  


**Sobre o Ibope** 

Os pontos percentuais de audiência são atualizados anualmente pela Kantar Ibope Media. Para ser ter uma noção de como funciona, na Grande São Paulo, por exemplo, 1 ponto de audiência representa 73.015 domicílios que assistiram a determinado programa, ou cerca de 200.766 pessoas. Já na capital do Rio de Janeiro, cada ponto equivale a 46.175 domicílios, ou 118.440 pessoas. Enquanto no "Painel Nacional de Televisão", cada ponto reflete 254.892 domicílios e 693.788 indivíduos.

Fonte: https://economia.uol.com.br/noticias/redacao/2019/12/06/como-sao-os-aparelhos-de-medicao-de-audiencia-do-ibope.htm

## Por onde começar?

![Carminha](https://media.giphy.com/media/3o84TWWmhqCJKpdbxK/giphy.gif)

**Primeiramente importamos as bibliotecas que iremos utilizar.**


```python
import pandas as pd
import plotly.graph_objects as go
import plotly.express as px
import seaborn as sns

```

**Depois importamos nosso dataset e iniciamos nossa análise exploratória.**


```python
df = pd.read_excel("Base Novelas 01.xlsx")
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
      <th>Novela</th>
      <th>Inicio</th>
      <th>Final</th>
      <th>Capitulos</th>
      <th>Autor</th>
      <th>Audiencia</th>
      <th>Faixa</th>
      <th>Minutos</th>
      <th>Horas</th>
      <th>Maiores</th>
      <th>Mais Famosas</th>
      <th>Audiência Média Por Autor</th>
      <th>Posição Autor</th>
      <th>Número de Paravras</th>
      <th>Posição Qtd Novelas</th>
      <th>Classificação</th>
      <th>Ano</th>
      <th>Menu</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Champagne</td>
      <td>1983-10-24</td>
      <td>1984-05-04</td>
      <td>167</td>
      <td>Cassiano Gabus Mendes</td>
      <td>46.84</td>
      <td>21 hrs</td>
      <td>8350</td>
      <td>139.166667</td>
      <td>155</td>
      <td>70</td>
      <td>50.644167</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>&gt;40</td>
      <td>1984</td>
      <td>01 - &gt;40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Meu Bem, Meu Mal</td>
      <td>1990-10-29</td>
      <td>1991-05-17</td>
      <td>173</td>
      <td>Cassiano Gabus Mendes</td>
      <td>50.34</td>
      <td>21 hrs</td>
      <td>8650</td>
      <td>144.166667</td>
      <td>135</td>
      <td>45</td>
      <td>50.644167</td>
      <td>1</td>
      <td>4</td>
      <td>7</td>
      <td>&gt;40</td>
      <td>1991</td>
      <td>01 - &gt;40</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Escalada</td>
      <td>1975-01-06</td>
      <td>1975-08-25</td>
      <td>199</td>
      <td>Lauro César Muniz</td>
      <td>47.96</td>
      <td>21 hrs</td>
      <td>9950</td>
      <td>165.833333</td>
      <td>62</td>
      <td>59</td>
      <td>48.451000</td>
      <td>3</td>
      <td>1</td>
      <td>11</td>
      <td>&gt;40</td>
      <td>1975</td>
      <td>01 - &gt;40</td>
    </tr>
    <tr>
      <th>3</th>
      <td>O Casarão</td>
      <td>1976-06-07</td>
      <td>1976-12-10</td>
      <td>159</td>
      <td>Lauro César Muniz</td>
      <td>51.82</td>
      <td>21 hrs</td>
      <td>7950</td>
      <td>132.500000</td>
      <td>188</td>
      <td>36</td>
      <td>48.451000</td>
      <td>3</td>
      <td>2</td>
      <td>11</td>
      <td>&gt;40</td>
      <td>1976</td>
      <td>01 - &gt;40</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Espelho Mágico</td>
      <td>1977-06-14</td>
      <td>1977-12-05</td>
      <td>150</td>
      <td>Lauro César Muniz</td>
      <td>48.91</td>
      <td>21 hrs</td>
      <td>7500</td>
      <td>125.000000</td>
      <td>216</td>
      <td>54</td>
      <td>48.451000</td>
      <td>3</td>
      <td>2</td>
      <td>11</td>
      <td>&gt;40</td>
      <td>1977</td>
      <td>01 - &gt;40</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (89, 18)



**Note que nosso dataset não possui dados faltantes, o que irá facilitar nossa análise.**


```python
df.isnull()
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
      <th>Novela</th>
      <th>Inicio</th>
      <th>Final</th>
      <th>Capitulos</th>
      <th>Autor</th>
      <th>Audiencia</th>
      <th>Faixa</th>
      <th>Minutos</th>
      <th>Horas</th>
      <th>Maiores</th>
      <th>Mais Famosas</th>
      <th>Audiência Média Por Autor</th>
      <th>Posição Autor</th>
      <th>Número de Paravras</th>
      <th>Posição Qtd Novelas</th>
      <th>Classificação</th>
      <th>Ano</th>
      <th>Menu</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>84</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>85</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>86</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>87</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>88</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>89 rows × 18 columns</p>
</div>




```python
df.dtypes
```




    Novela                               object
    Inicio                       datetime64[ns]
    Final                        datetime64[ns]
    Capitulos                             int64
    Autor                                object
    Audiencia                           float64
    Faixa                                object
    Minutos                               int64
    Horas                               float64
    Maiores                               int64
    Mais Famosas                          int64
    Audiência Média Por Autor           float64
    Posição Autor                         int64
    Número de Paravras                    int64
    Posição Qtd Novelas                   int64
    Classificação                        object
    Ano                                   int64
    Menu                                 object
    dtype: object



**Aqui começamos a tirar alguns insights estatísticos bem relevantes.** 

![burrinho](https://media.giphy.com/media/RKpEvhcmdY4sHrlUAJ/giphy.gif)

    1- A menor audiência histórica desde 1966 foi de apenas 5.23 pontos.
    2- Enquanto a máxima histórica foi de 64.97 pontos.
    3- Outro fator interessante é o tempo médio de exibição, de aproximadamente 152 horas.



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
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Capitulos</th>
      <th>Audiencia</th>
      <th>Minutos</th>
      <th>Horas</th>
      <th>Maiores</th>
      <th>Mais Famosas</th>
      <th>Audiência Média Por Autor</th>
      <th>Posição Autor</th>
      <th>Número de Paravras</th>
      <th>Posição Qtd Novelas</th>
      <th>Ano</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.00000</td>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.000000</td>
      <td>89.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>182.696629</td>
      <td>43.529551</td>
      <td>9134.831461</td>
      <td>152.247191</td>
      <td>107.58427</td>
      <td>96.730337</td>
      <td>40.993471</td>
      <td>15.235955</td>
      <td>2.449438</td>
      <td>7.640449</td>
      <td>1992.584270</td>
    </tr>
    <tr>
      <th>std</th>
      <td>36.886929</td>
      <td>13.462116</td>
      <td>1844.346467</td>
      <td>30.739108</td>
      <td>79.72122</td>
      <td>82.429827</td>
      <td>8.891376</td>
      <td>14.699275</td>
      <td>1.000128</td>
      <td>7.669668</td>
      <td>15.594149</td>
    </tr>
    <tr>
      <th>min</th>
      <td>35.000000</td>
      <td>5.230000</td>
      <td>1750.000000</td>
      <td>29.166667</td>
      <td>2.00000</td>
      <td>1.000000</td>
      <td>4.135000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1966.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>167.000000</td>
      <td>37.950000</td>
      <td>8350.000000</td>
      <td>139.166667</td>
      <td>48.00000</td>
      <td>33.000000</td>
      <td>40.866667</td>
      <td>8.000000</td>
      <td>2.000000</td>
      <td>4.000000</td>
      <td>1980.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>185.000000</td>
      <td>46.780000</td>
      <td>9250.000000</td>
      <td>154.166667</td>
      <td>79.00000</td>
      <td>72.000000</td>
      <td>41.804706</td>
      <td>11.000000</td>
      <td>2.000000</td>
      <td>5.000000</td>
      <td>1992.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>203.000000</td>
      <td>52.150000</td>
      <td>10150.000000</td>
      <td>169.166667</td>
      <td>155.00000</td>
      <td>137.000000</td>
      <td>46.068462</td>
      <td>14.000000</td>
      <td>3.000000</td>
      <td>11.000000</td>
      <td>2006.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>301.000000</td>
      <td>64.970000</td>
      <td>15050.000000</td>
      <td>250.833333</td>
      <td>305.00000</td>
      <td>302.000000</td>
      <td>50.644167</td>
      <td>73.000000</td>
      <td>5.000000</td>
      <td>45.000000</td>
      <td>2019.000000</td>
    </tr>
  </tbody>
</table>
</div>



**Uma vez que temos estas informações, que tal plotarmos  
uma linha histórica, identificando a média de audiência para dada período de 1966 a 2019?**


```python
audiencia_ano = df[["Audiencia","Ano"]].groupby(["Ano"]).mean()
```


```python
audiencia_ano.reset_index(inplace=True)

```


```python
audiencia_ano.head()
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
      <th>Ano</th>
      <th>Audiencia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1966</td>
      <td>5.230000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1967</td>
      <td>6.723333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1968</td>
      <td>10.740000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1969</td>
      <td>16.760000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1970</td>
      <td>31.560000</td>
    </tr>
  </tbody>
</table>
</div>



Observe que com o **Plotly**, muito diferente do matplotlib e seaborn, podemos interagir com o gráfico ao passar o mouse por ele, identificando assim o ano exato e sua respectiva pontuação de audiência.

![bigbrother](https://media.giphy.com/media/l3q2vTVSTJ4ra9FZu/giphy.gif)


```python
# Create figure and add line
fig = go.Figure()
fig.add_trace(go.Scatter(
    x=audiencia_ano["Ano"],
    y=audiencia_ano["Audiencia"],
    mode="lines"
))

# Set custom x-axis labels
fig.update_xaxes(showgrid=False)
fig.update_yaxes(showgrid=False)

# Set figure title
fig.update_layout(title_text="Audiência histórica")

fig.show()
```


<div>


            <div id="7faabb0f-282f-496b-89ab-e4fc54002308" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("7faabb0f-282f-496b-89ab-e4fc54002308")) {
                    Plotly.newPlot(
                        '7faabb0f-282f-496b-89ab-e4fc54002308',
                        [{"mode": "lines", "type": "scatter", "x": [1966, 1967, 1968, 1969, 1970, 1971, 1972, 1973, 1974, 1975, 1976, 1977, 1978, 1979, 1980, 1981, 1982, 1983, 1984, 1985, 1986, 1987, 1988, 1989, 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019], "y": [5.23, 6.723333333333333, 10.74, 16.76, 31.56, 48.2, 43.95, 49.59, 47.59, 50.055, 53.695, 53.175, 52.63, 60.8, 53.644999999999996, 56.195, 45.3, 47.155, 46.85, 52.67, 60.95, 56.235, 55.69, 61.59, 62.94, 50.34, 48.85, 56.805, 53.44, 46.51, 47.09, 49.83, 42.52, 40.835, 43.72, 44.629999999999995, 46.78, 42.225, 46.05, 49.760000000000005, 48.43, 44.980000000000004, 41.04, 39.010000000000005, 35.65, 35.455, 38.875, 33.97, 32.58, 29.089999999999996, 28.795, 31.439999999999998, 35.795, 28.78]}],
                        {"template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "title": {"text": "Audi\u00eancia hist\u00f3rica"}, "xaxis": {"showgrid": false}, "yaxis": {"showgrid": false}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('7faabb0f-282f-496b-89ab-e4fc54002308');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
fig.write_html("pontos-historico.html")
```

**Incrível como a visualização tem o poder de explicar muita coisa.**

Ao observarmos o gráfico de audiência histórica, já é possível reparar que desde 1990 a audiência das novelas das 21h segue em tendência de baixa, sendo que a audiência atual (2019) se aproxima, e muito, do patamar apresentado em 1970.

Algo já começa a ficar evidente; O período de ouro das novelas das nove, na Rede Globo aconteceu nos anos de 1979, 1986, 1989 e 1990, onde ela conseguiu em alguns momentos atingir incríveis picos entre 60 e 63 pontos de audiência.

A diferença é que no passado, havia apenas 5 anos que a Rede Globo iniciara sua transmissão de novelas às nove da noite e agora, 54 anos depois, apresenta mesmos quase trinta pontos do início de sua empreitada no segmentos.

![Cauan](https://media.giphy.com/media/l1rg1bac9gaxo3q3V3/giphy.gif)

**Agora que tal analisarmos quais foram os autores que alcançaram a maior audiência da história?**

Para isso, existem vários métodos, mas decidi utilizar o groupby para selecionar somente duas colunas e retornar, a quantidade de novelas escritas por autor.


```python
autor_novelas = df[["Posição Qtd Novelas","Autor"]].groupby(["Autor"]).mean()
```


```python
autor_novelas.reset_index(inplace=True)
```


```python
autor_novelas
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
      <th>Autor</th>
      <th>Posição Qtd Novelas</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aguinaldo Silva</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Benedito Ruy Barbosa</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cassiano Gabus Mendes</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Dias Gomes</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Emiliano Queiroz</td>
      <td>45</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Gilberto Braga</td>
      <td>4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Glória Magadan</td>
      <td>12</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Glória Perez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Janete Clair</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>José Castellar</td>
      <td>45</td>
    </tr>
    <tr>
      <th>10</th>
      <td>João Emanuel Carneiro</td>
      <td>17</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Lauro César Muniz</td>
      <td>11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Manoel Carlos</td>
      <td>7</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Maria Adelaide Amaral</td>
      <td>21</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Moysés Weltman</td>
      <td>21</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Regina Braga</td>
      <td>30</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Sílvio de Abreu</td>
      <td>7</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Walcyr Carrasco</td>
      <td>7</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Walther Negrão</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



A primeira coisa que reparamos é que entre todos os autores encontramos o que em ciencia de dados chamamos de outliers(Anomalias).
Ou seja, um número minúsculo de autores completou a marca de 45 novelas das nove escritas durante sua carreira.  
Enquanto a maior parte dos autores de novelas das nove escreveram entre 5 e 20 novelas.


```python
fig = px.box(autor_novelas, y="Posição Qtd Novelas")
fig.show()
```


<div>


            <div id="e6879a88-2ee5-4f43-934e-a4ffb7f1091c" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("e6879a88-2ee5-4f43-934e-a4ffb7f1091c")) {
                    Plotly.newPlot(
                        'e6879a88-2ee5-4f43-934e-a4ffb7f1091c',
                        [{"alignmentgroup": "True", "hovertemplate": "Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "", "marker": {"color": "#636efa"}, "name": "", "notched": false, "offsetgroup": "", "orientation": "v", "showlegend": false, "type": "box", "x0": " ", "xaxis": "x", "y": [5, 2, 7, 5, 45, 4, 12, 12, 2, 45, 17, 11, 7, 21, 21, 30, 7, 7, 1], "y0": " ", "yaxis": "y"}],
                        {"boxmode": "group", "legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0]}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Posi\u00e7\u00e3o Qtd Novelas"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('e6879a88-2ee5-4f43-934e-a4ffb7f1091c');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
fig.write_html("posicao_qtd_novelas.html")
```

**Aqui plotamos a distribuição de autores pela quantidade de novelas das nove escritas.**


```python
fig = px.histogram(autor_novelas, x="Autor", y="Posição Qtd Novelas", color="Autor")
fig.show()
```


<div>


            <div id="00745a89-e52e-4d35-be65-d4f550d8eece" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("00745a89-e52e-4d35-be65-d4f550d8eece")) {
                    Plotly.newPlot(
                        '00745a89-e52e-4d35-be65-d4f550d8eece',
                        [{"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Aguinaldo Silva", "marker": {"color": "#636efa"}, "name": "Aguinaldo Silva", "offsetgroup": "Aguinaldo Silva", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Aguinaldo Silva"], "xaxis": "x", "y": [5], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Benedito Ruy Barbosa", "marker": {"color": "#EF553B"}, "name": "Benedito Ruy Barbosa", "offsetgroup": "Benedito Ruy Barbosa", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Benedito Ruy Barbosa"], "xaxis": "x", "y": [2], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Cassiano Gabus Mendes", "marker": {"color": "#00cc96"}, "name": "Cassiano Gabus Mendes", "offsetgroup": "Cassiano Gabus Mendes", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Cassiano Gabus Mendes"], "xaxis": "x", "y": [7], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Dias Gomes", "marker": {"color": "#ab63fa"}, "name": "Dias Gomes", "offsetgroup": "Dias Gomes", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Dias Gomes"], "xaxis": "x", "y": [5], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Emiliano Queiroz", "marker": {"color": "#FFA15A"}, "name": "Emiliano Queiroz", "offsetgroup": "Emiliano Queiroz", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Emiliano Queiroz"], "xaxis": "x", "y": [45], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Gilberto Braga", "marker": {"color": "#19d3f3"}, "name": "Gilberto Braga", "offsetgroup": "Gilberto Braga", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Gilberto Braga"], "xaxis": "x", "y": [4], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Gl\u00f3ria Magadan", "marker": {"color": "#FF6692"}, "name": "Gl\u00f3ria Magadan", "offsetgroup": "Gl\u00f3ria Magadan", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Gl\u00f3ria Magadan"], "xaxis": "x", "y": [12], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Gl\u00f3ria Perez", "marker": {"color": "#B6E880"}, "name": "Gl\u00f3ria Perez", "offsetgroup": "Gl\u00f3ria Perez", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Gl\u00f3ria Perez"], "xaxis": "x", "y": [12], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Janete Clair", "marker": {"color": "#FF97FF"}, "name": "Janete Clair", "offsetgroup": "Janete Clair", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Janete Clair"], "xaxis": "x", "y": [2], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Jos\u00e9 Castellar", "marker": {"color": "#FECB52"}, "name": "Jos\u00e9 Castellar", "offsetgroup": "Jos\u00e9 Castellar", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Jos\u00e9 Castellar"], "xaxis": "x", "y": [45], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Jo\u00e3o Emanuel Carneiro", "marker": {"color": "#636efa"}, "name": "Jo\u00e3o Emanuel Carneiro", "offsetgroup": "Jo\u00e3o Emanuel Carneiro", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Jo\u00e3o Emanuel Carneiro"], "xaxis": "x", "y": [17], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Lauro C\u00e9sar Muniz", "marker": {"color": "#EF553B"}, "name": "Lauro C\u00e9sar Muniz", "offsetgroup": "Lauro C\u00e9sar Muniz", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Lauro C\u00e9sar Muniz"], "xaxis": "x", "y": [11], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Manoel Carlos", "marker": {"color": "#00cc96"}, "name": "Manoel Carlos", "offsetgroup": "Manoel Carlos", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Manoel Carlos"], "xaxis": "x", "y": [7], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Maria Adelaide Amaral", "marker": {"color": "#ab63fa"}, "name": "Maria Adelaide Amaral", "offsetgroup": "Maria Adelaide Amaral", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Maria Adelaide Amaral"], "xaxis": "x", "y": [21], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Moys\u00e9s Weltman", "marker": {"color": "#FFA15A"}, "name": "Moys\u00e9s Weltman", "offsetgroup": "Moys\u00e9s Weltman", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Moys\u00e9s Weltman"], "xaxis": "x", "y": [21], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Regina Braga", "marker": {"color": "#19d3f3"}, "name": "Regina Braga", "offsetgroup": "Regina Braga", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Regina Braga"], "xaxis": "x", "y": [30], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "S\u00edlvio de Abreu", "marker": {"color": "#FF6692"}, "name": "S\u00edlvio de Abreu", "offsetgroup": "S\u00edlvio de Abreu", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["S\u00edlvio de Abreu"], "xaxis": "x", "y": [7], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Walcyr Carrasco", "marker": {"color": "#B6E880"}, "name": "Walcyr Carrasco", "offsetgroup": "Walcyr Carrasco", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Walcyr Carrasco"], "xaxis": "x", "y": [7], "yaxis": "y"}, {"alignmentgroup": "True", "bingroup": "x", "histfunc": "sum", "hovertemplate": "Autor=%{x}<br>sum of Posi\u00e7\u00e3o Qtd Novelas=%{y}<extra></extra>", "legendgroup": "Walther Negr\u00e3o", "marker": {"color": "#FF97FF"}, "name": "Walther Negr\u00e3o", "offsetgroup": "Walther Negr\u00e3o", "orientation": "v", "showlegend": true, "type": "histogram", "x": ["Walther Negr\u00e3o"], "xaxis": "x", "y": [1], "yaxis": "y"}],
                        {"barmode": "relative", "legend": {"title": {"text": "Autor"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "categoryarray": ["Aguinaldo Silva", "Benedito Ruy Barbosa", "Cassiano Gabus Mendes", "Dias Gomes", "Emiliano Queiroz", "Gilberto Braga", "Gl\u00f3ria Magadan", "Gl\u00f3ria Perez", "Janete Clair", "Jos\u00e9 Castellar", "Jo\u00e3o Emanuel Carneiro", "Lauro C\u00e9sar Muniz", "Manoel Carlos", "Maria Adelaide Amaral", "Moys\u00e9s Weltman", "Regina Braga", "S\u00edlvio de Abreu", "Walcyr Carrasco", "Walther Negr\u00e3o"], "categoryorder": "array", "domain": [0.0, 1.0], "title": {"text": "Autor"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "sum of Posi\u00e7\u00e3o Qtd Novelas"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('00745a89-e52e-4d35-be65-d4f550d8eece');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>


**Agora a coisa começa a ficar interessante. Por meio do Scatterplot podemos plotar um gráfico que nos mostre  
a distribuição de novelas e sua respectiva audiência, segmentada cores. Sendo assim, cada artista possui a sua cor.**

Ao passar o mouse sobre o gráfico ou selecionar os autores na legenda, você pode aprimorar sua visualização,  
conseguindo não só comparar autores entre si, mas também qual a novela de determinado autor obteve maior audiência. 
Incrível, né?

![Ivete](https://media.giphy.com/media/l0ExlaDotIC3rw1Og/giphy.gif)


```python
fig = px.scatter(df, x="Novela", y="Audiencia", color="Autor")
fig.show()
```


<div>


            <div id="c0511d9c-8cc9-406e-b408-ab05de0cace7" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("c0511d9c-8cc9-406e-b408-ab05de0cace7")) {
                    Plotly.newPlot(
                        'c0511d9c-8cc9-406e-b408-ab05de0cace7',
                        [{"hovertemplate": "Autor=Cassiano Gabus Mendes<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Cassiano Gabus Mendes", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Cassiano Gabus Mendes", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Champagne", "Meu Bem, Meu Mal"], "xaxis": "x", "y": [46.84, 50.34], "yaxis": "y"}, {"hovertemplate": "Autor=Lauro C\u00e9sar Muniz<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Lauro C\u00e9sar Muniz", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Lauro C\u00e9sar Muniz", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Escalada", "O Casar\u00e3o", "Espelho M\u00e1gico", "Os Gigantes", "Roda de Fogo", "O Salvador da P\u00e1tria"], "xaxis": "x", "y": [47.96, 51.82, 48.91, 49.29, 55.46, 62.05], "yaxis": "y"}, {"hovertemplate": "Autor=Regina Braga<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Regina Braga", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Regina Braga", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Selva de Pedra"], "xaxis": "x", "y": [59.6], "yaxis": "y"}, {"hovertemplate": "Autor=S\u00edlvio de Abreu<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "S\u00edlvio de Abreu", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "S\u00edlvio de Abreu", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Rainha da Sucata", "A Pr\u00f3xima V\u00edtima", "Torre de Babel", "Bel\u00edssima", "Passione"], "xaxis": "x", "y": [60.91, 47.41, 43.56, 48.43, 35.13], "yaxis": "y"}, {"hovertemplate": "Autor=Aguinaldo Silva<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Aguinaldo Silva", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Aguinaldo Silva", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Partido Alto", "O Outro", "Tieta", "Pedra sobre Pedra", "Fera Ferida", "A Indomada", "Suave Veneno", "Porto dos Milagres", "Senhora do Destino", "Duas Caras", "Fina Estampa", "Imp\u00e9rio", "O S\u00e9timo Guardi\u00e3o"], "xaxis": "x", "y": [46.86, 57.01, 64.97, 54.39, 53.44, 47.85, 38.11, 44.36, 50.31, 41.04, 39.04, 32.73, 28.78], "yaxis": "y"}, {"hovertemplate": "Autor=Gilberto Braga<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Gilberto Braga", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Gilberto Braga", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Dancin\u2019 Days", "\u00c1gua Viva", "Brilhante", "Louco Amor", "Corpo a Corpo", "Vale Tudo", "O Dono do Mundo", "P\u00e1tria Minha", "Celebridade", "Para\u00edso Tropical", "Insensato Cora\u00e7\u00e3o", "Babil\u00f4nia"], "xaxis": "x", "y": [60.0, 58.0, 44.64, 49.51, 52.67, 61.13, 43.31, 45.61, 46.05, 42.88, 35.78, 25.45], "yaxis": "y"}, {"hovertemplate": "Autor=Janete Clair<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Janete Clair", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Janete Clair", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Sangue e Areia", "Passo dos Ventos", "Rosa Rebelde", "V\u00e9u de Noiva", "Irm\u00e3os Coragem", "O Homem que Deve Morrer", "Selva de Pedra", "O Semideus", "Fogo sobre Terra", "Pecado Capital", "Duas Vidas", "O Astro", "Pai Her\u00f3i", "Cora\u00e7\u00e3o Alado", "S\u00e9timo Sentido"], "xaxis": "x", "y": [10.74, 11.56, 21.96, 31.56, 48.2, 43.95, 48.68, 47.59, 52.15, 55.57, 57.44, 52.63, 61.6, 56.88, 45.96], "yaxis": "y"}, {"hovertemplate": "Autor=Manoel Carlos<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Manoel Carlos", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Manoel Carlos", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Baila Comigo", "Sol de Ver\u00e3o", "Por Amor", "La\u00e7os de Fam\u00edlia", "Mulheres Apaixonadas", "P\u00e1ginas da Vida", "Viver a Vida", "Em Fam\u00edlia"], "xaxis": "x", "y": [55.51, 44.8, 42.52, 44.9, 46.5, 47.08, 35.65, 29.65], "yaxis": "y"}, {"hovertemplate": "Autor=Gl\u00f3ria Perez<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Gl\u00f3ria Perez", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Gl\u00f3ria Perez", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["De Corpo e Alma", "Explode Cora\u00e7\u00e3o", "O Clone", "Am\u00e9rica", "Caminho das \u00cdndias", "Salve Jorge", "A For\u00e7a do Querer"], "xaxis": "x", "y": [52.72, 47.37, 46.78, 49.21, 38.59, 33.97, 35.66], "yaxis": "y"}, {"hovertemplate": "Autor=Benedito Ruy Barbosa<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Benedito Ruy Barbosa", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "Benedito Ruy Barbosa", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Renascer", "O Rei do Gado", "Terra Nostra", "Esperan\u00e7a", "Velho Chico"], "xaxis": "x", "y": [60.89, 51.81, 43.72, 37.95, 29.05], "yaxis": "y"}, {"hovertemplate": "Autor=Jo\u00e3o Emanuel Carneiro<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Jo\u00e3o Emanuel Carneiro", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Jo\u00e3o Emanuel Carneiro", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["A Favorita", "Avenida Brasil", "A Regra do Jogo", "Segundo Sol"], "xaxis": "x", "y": [39.43, 38.71, 28.54, 33.36], "yaxis": "y"}, {"hovertemplate": "Autor=Walther Negr\u00e3o<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Walther Negr\u00e3o", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Walther Negr\u00e3o", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Cavalo de A\u00e7o"], "xaxis": "x", "y": [50.5], "yaxis": "y"}, {"hovertemplate": "Autor=Dias Gomes<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Dias Gomes", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Dias Gomes", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Roque Santeiro", "Mandala", "O Fim do Mundo"], "xaxis": "x", "y": [62.3, 55.69, 46.81], "yaxis": "y"}, {"hovertemplate": "Autor=Walcyr Carrasco<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Walcyr Carrasco", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "Walcyr Carrasco", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Amor \u00e0 Vida", "O Outro Lado do Para\u00edso"], "xaxis": "x", "y": [35.51, 38.23], "yaxis": "y"}, {"hovertemplate": "Autor=Maria Adelaide Amaral<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Maria Adelaide Amaral", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Maria Adelaide Amaral", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["A Lei do Amor"], "xaxis": "x", "y": [27.22], "yaxis": "y"}, {"hovertemplate": "Autor=Gl\u00f3ria Magadan<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Gl\u00f3ria Magadan", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Gl\u00f3ria Magadan", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["A Sombra da Rebecca"], "xaxis": "x", "y": [8.0], "yaxis": "y"}, {"hovertemplate": "Autor=Emiliano Queiroz<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Emiliano Queiroz", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Emiliano Queiroz", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["Anast\u00e1cia, a Mulher sem Destino"], "xaxis": "x", "y": [5.47], "yaxis": "y"}, {"hovertemplate": "Autor=Jos\u00e9 Castellar<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Jos\u00e9 Castellar", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Jos\u00e9 Castellar", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["O \u00c9brio"], "xaxis": "x", "y": [5.23], "yaxis": "y"}, {"hovertemplate": "Autor=Moys\u00e9s Weltman<br>Novela=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Moys\u00e9s Weltman", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Moys\u00e9s Weltman", "orientation": "v", "showlegend": true, "type": "scatter", "x": ["O Rei dos Ciganos"], "xaxis": "x", "y": [6.7], "yaxis": "y"}],
                        {"legend": {"title": {"text": "Autor"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "Novela"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Audiencia"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('c0511d9c-8cc9-406e-b408-ab05de0cace7');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
fig.write_html("audiencia-novela.html")
```

**Mas afinal de contas, qual novela conquistou a maior audiência até hoje?**

Ao visualizar o gráfico abaixo notamos que a maior pontuação da história foi alcançada pela novela **Tieta**, de **1990**. Ela chegou a bater **64.97 pontos**.

Desde 2010 não se consegue ultrapassar marca de **Fina Estampa**, com **39.04 pontos** de audiência.


```python
fig = px.scatter(df, x="Ano", y="Audiencia", color="Novela")
fig.show()
```


<div>


            <div id="3450d477-8d7e-43f9-b0b6-9e63f62daf2b" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("3450d477-8d7e-43f9-b0b6-9e63f62daf2b")) {
                    Plotly.newPlot(
                        '3450d477-8d7e-43f9-b0b6-9e63f62daf2b',
                        [{"hovertemplate": "Novela=Champagne<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Champagne", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Champagne", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1984], "xaxis": "x", "y": [46.84], "yaxis": "y"}, {"hovertemplate": "Novela=Meu Bem, Meu Mal<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Meu Bem, Meu Mal", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Meu Bem, Meu Mal", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1991], "xaxis": "x", "y": [50.34], "yaxis": "y"}, {"hovertemplate": "Novela=Escalada<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Escalada", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Escalada", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1975], "xaxis": "x", "y": [47.96], "yaxis": "y"}, {"hovertemplate": "Novela=O Casar\u00e3o<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Casar\u00e3o", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "O Casar\u00e3o", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1976], "xaxis": "x", "y": [51.82], "yaxis": "y"}, {"hovertemplate": "Novela=Espelho M\u00e1gico<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Espelho M\u00e1gico", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Espelho M\u00e1gico", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1977], "xaxis": "x", "y": [48.91], "yaxis": "y"}, {"hovertemplate": "Novela=Os Gigantes<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Os Gigantes", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Os Gigantes", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1980], "xaxis": "x", "y": [49.29], "yaxis": "y"}, {"hovertemplate": "Novela=Roda de Fogo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Roda de Fogo", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Roda de Fogo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1987], "xaxis": "x", "y": [55.46], "yaxis": "y"}, {"hovertemplate": "Novela=O Salvador da P\u00e1tria<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Salvador da P\u00e1tria", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "O Salvador da P\u00e1tria", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1989], "xaxis": "x", "y": [62.05], "yaxis": "y"}, {"hovertemplate": "Novela=Selva de Pedra<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Selva de Pedra", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Selva de Pedra", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1986, 1973], "xaxis": "x", "y": [59.6, 48.68], "yaxis": "y"}, {"hovertemplate": "Novela=Rainha da Sucata<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Rainha da Sucata", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "Rainha da Sucata", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1990], "xaxis": "x", "y": [60.91], "yaxis": "y"}, {"hovertemplate": "Novela=A Pr\u00f3xima V\u00edtima<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A Pr\u00f3xima V\u00edtima", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "A Pr\u00f3xima V\u00edtima", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1995], "xaxis": "x", "y": [47.41], "yaxis": "y"}, {"hovertemplate": "Novela=Torre de Babel<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Torre de Babel", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Torre de Babel", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1999], "xaxis": "x", "y": [43.56], "yaxis": "y"}, {"hovertemplate": "Novela=Bel\u00edssima<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Bel\u00edssima", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Bel\u00edssima", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2006], "xaxis": "x", "y": [48.43], "yaxis": "y"}, {"hovertemplate": "Novela=Passione<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Passione", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "Passione", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2011], "xaxis": "x", "y": [35.13], "yaxis": "y"}, {"hovertemplate": "Novela=Partido Alto<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Partido Alto", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Partido Alto", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1984], "xaxis": "x", "y": [46.86], "yaxis": "y"}, {"hovertemplate": "Novela=O Outro<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Outro", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "O Outro", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1987], "xaxis": "x", "y": [57.01], "yaxis": "y"}, {"hovertemplate": "Novela=Tieta<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Tieta", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Tieta", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1990], "xaxis": "x", "y": [64.97], "yaxis": "y"}, {"hovertemplate": "Novela=Pedra sobre Pedra<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Pedra sobre Pedra", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Pedra sobre Pedra", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1992], "xaxis": "x", "y": [54.39], "yaxis": "y"}, {"hovertemplate": "Novela=Fera Ferida<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Fera Ferida", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Fera Ferida", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1994], "xaxis": "x", "y": [53.44], "yaxis": "y"}, {"hovertemplate": "Novela=A Indomada<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A Indomada", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "A Indomada", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1997], "xaxis": "x", "y": [47.85], "yaxis": "y"}, {"hovertemplate": "Novela=Suave Veneno<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Suave Veneno", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Suave Veneno", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1999], "xaxis": "x", "y": [38.11], "yaxis": "y"}, {"hovertemplate": "Novela=Porto dos Milagres<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Porto dos Milagres", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Porto dos Milagres", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2001], "xaxis": "x", "y": [44.36], "yaxis": "y"}, {"hovertemplate": "Novela=Senhora do Destino<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Senhora do Destino", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Senhora do Destino", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2005], "xaxis": "x", "y": [50.31], "yaxis": "y"}, {"hovertemplate": "Novela=Duas Caras<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Duas Caras", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "Duas Caras", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2008], "xaxis": "x", "y": [41.04], "yaxis": "y"}, {"hovertemplate": "Novela=Fina Estampa<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Fina Estampa", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Fina Estampa", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2012], "xaxis": "x", "y": [39.04], "yaxis": "y"}, {"hovertemplate": "Novela=Imp\u00e9rio<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Imp\u00e9rio", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Imp\u00e9rio", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2015], "xaxis": "x", "y": [32.73], "yaxis": "y"}, {"hovertemplate": "Novela=O S\u00e9timo Guardi\u00e3o<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O S\u00e9timo Guardi\u00e3o", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "O S\u00e9timo Guardi\u00e3o", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2019], "xaxis": "x", "y": [28.78], "yaxis": "y"}, {"hovertemplate": "Novela=Dancin\u2019 Days<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Dancin\u2019 Days", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Dancin\u2019 Days", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1979], "xaxis": "x", "y": [60.0], "yaxis": "y"}, {"hovertemplate": "Novela=\u00c1gua Viva<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "\u00c1gua Viva", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "\u00c1gua Viva", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1980], "xaxis": "x", "y": [58.0], "yaxis": "y"}, {"hovertemplate": "Novela=Brilhante<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Brilhante", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "Brilhante", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1982], "xaxis": "x", "y": [44.64], "yaxis": "y"}, {"hovertemplate": "Novela=Louco Amor<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Louco Amor", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Louco Amor", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1983], "xaxis": "x", "y": [49.51], "yaxis": "y"}, {"hovertemplate": "Novela=Corpo a Corpo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Corpo a Corpo", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Corpo a Corpo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1985], "xaxis": "x", "y": [52.67], "yaxis": "y"}, {"hovertemplate": "Novela=Vale Tudo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Vale Tudo", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Vale Tudo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1989], "xaxis": "x", "y": [61.13], "yaxis": "y"}, {"hovertemplate": "Novela=O Dono do Mundo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Dono do Mundo", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "O Dono do Mundo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1992], "xaxis": "x", "y": [43.31], "yaxis": "y"}, {"hovertemplate": "Novela=P\u00e1tria Minha<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "P\u00e1tria Minha", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "P\u00e1tria Minha", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1995], "xaxis": "x", "y": [45.61], "yaxis": "y"}, {"hovertemplate": "Novela=Celebridade<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Celebridade", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Celebridade", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2004], "xaxis": "x", "y": [46.05], "yaxis": "y"}, {"hovertemplate": "Novela=Para\u00edso Tropical<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Para\u00edso Tropical", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Para\u00edso Tropical", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2007], "xaxis": "x", "y": [42.88], "yaxis": "y"}, {"hovertemplate": "Novela=Insensato Cora\u00e7\u00e3o<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Insensato Cora\u00e7\u00e3o", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Insensato Cora\u00e7\u00e3o", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2011], "xaxis": "x", "y": [35.78], "yaxis": "y"}, {"hovertemplate": "Novela=Babil\u00f4nia<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Babil\u00f4nia", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Babil\u00f4nia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2015], "xaxis": "x", "y": [25.45], "yaxis": "y"}, {"hovertemplate": "Novela=Sangue e Areia<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Sangue e Areia", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "Sangue e Areia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1968], "xaxis": "x", "y": [10.74], "yaxis": "y"}, {"hovertemplate": "Novela=Passo dos Ventos<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Passo dos Ventos", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Passo dos Ventos", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1969], "xaxis": "x", "y": [11.56], "yaxis": "y"}, {"hovertemplate": "Novela=Rosa Rebelde<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Rosa Rebelde", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Rosa Rebelde", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1969], "xaxis": "x", "y": [21.96], "yaxis": "y"}, {"hovertemplate": "Novela=V\u00e9u de Noiva<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "V\u00e9u de Noiva", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "V\u00e9u de Noiva", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1970], "xaxis": "x", "y": [31.56], "yaxis": "y"}, {"hovertemplate": "Novela=Irm\u00e3os Coragem<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Irm\u00e3os Coragem", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "Irm\u00e3os Coragem", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1971], "xaxis": "x", "y": [48.2], "yaxis": "y"}, {"hovertemplate": "Novela=O Homem que Deve Morrer<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Homem que Deve Morrer", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "O Homem que Deve Morrer", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1972], "xaxis": "x", "y": [43.95], "yaxis": "y"}, {"hovertemplate": "Novela=O Semideus<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Semideus", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "O Semideus", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1974], "xaxis": "x", "y": [47.59], "yaxis": "y"}, {"hovertemplate": "Novela=Fogo sobre Terra<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Fogo sobre Terra", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Fogo sobre Terra", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1975], "xaxis": "x", "y": [52.15], "yaxis": "y"}, {"hovertemplate": "Novela=Pecado Capital<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Pecado Capital", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Pecado Capital", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1976], "xaxis": "x", "y": [55.57], "yaxis": "y"}, {"hovertemplate": "Novela=Duas Vidas<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Duas Vidas", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Duas Vidas", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1977], "xaxis": "x", "y": [57.44], "yaxis": "y"}, {"hovertemplate": "Novela=O Astro<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Astro", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "O Astro", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1978], "xaxis": "x", "y": [52.63], "yaxis": "y"}, {"hovertemplate": "Novela=Pai Her\u00f3i<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Pai Her\u00f3i", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Pai Her\u00f3i", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1979], "xaxis": "x", "y": [61.6], "yaxis": "y"}, {"hovertemplate": "Novela=Cora\u00e7\u00e3o Alado<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Cora\u00e7\u00e3o Alado", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Cora\u00e7\u00e3o Alado", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1981], "xaxis": "x", "y": [56.88], "yaxis": "y"}, {"hovertemplate": "Novela=S\u00e9timo Sentido<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "S\u00e9timo Sentido", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "S\u00e9timo Sentido", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1982], "xaxis": "x", "y": [45.96], "yaxis": "y"}, {"hovertemplate": "Novela=Baila Comigo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Baila Comigo", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "Baila Comigo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1981], "xaxis": "x", "y": [55.51], "yaxis": "y"}, {"hovertemplate": "Novela=Sol de Ver\u00e3o<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Sol de Ver\u00e3o", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Sol de Ver\u00e3o", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1983], "xaxis": "x", "y": [44.8], "yaxis": "y"}, {"hovertemplate": "Novela=Por Amor<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Por Amor", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Por Amor", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1998], "xaxis": "x", "y": [42.52], "yaxis": "y"}, {"hovertemplate": "Novela=La\u00e7os de Fam\u00edlia<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "La\u00e7os de Fam\u00edlia", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "La\u00e7os de Fam\u00edlia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2001], "xaxis": "x", "y": [44.9], "yaxis": "y"}, {"hovertemplate": "Novela=Mulheres Apaixonadas<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Mulheres Apaixonadas", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Mulheres Apaixonadas", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2003], "xaxis": "x", "y": [46.5], "yaxis": "y"}, {"hovertemplate": "Novela=P\u00e1ginas da Vida<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "P\u00e1ginas da Vida", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "P\u00e1ginas da Vida", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2007], "xaxis": "x", "y": [47.08], "yaxis": "y"}, {"hovertemplate": "Novela=Viver a Vida<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Viver a Vida", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "Viver a Vida", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2010], "xaxis": "x", "y": [35.65], "yaxis": "y"}, {"hovertemplate": "Novela=Em Fam\u00edlia<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Em Fam\u00edlia", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Em Fam\u00edlia", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2014], "xaxis": "x", "y": [29.65], "yaxis": "y"}, {"hovertemplate": "Novela=De Corpo e Alma<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "De Corpo e Alma", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "De Corpo e Alma", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1993], "xaxis": "x", "y": [52.72], "yaxis": "y"}, {"hovertemplate": "Novela=Explode Cora\u00e7\u00e3o<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Explode Cora\u00e7\u00e3o", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Explode Cora\u00e7\u00e3o", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1996], "xaxis": "x", "y": [47.37], "yaxis": "y"}, {"hovertemplate": "Novela=O Clone<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Clone", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "O Clone", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2002], "xaxis": "x", "y": [46.78], "yaxis": "y"}, {"hovertemplate": "Novela=Am\u00e9rica<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Am\u00e9rica", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Am\u00e9rica", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2005], "xaxis": "x", "y": [49.21], "yaxis": "y"}, {"hovertemplate": "Novela=Caminho das \u00cdndias<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Caminho das \u00cdndias", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Caminho das \u00cdndias", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2009], "xaxis": "x", "y": [38.59], "yaxis": "y"}, {"hovertemplate": "Novela=Salve Jorge<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Salve Jorge", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Salve Jorge", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2013], "xaxis": "x", "y": [33.97], "yaxis": "y"}, {"hovertemplate": "Novela=A For\u00e7a do Querer<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A For\u00e7a do Querer", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "A For\u00e7a do Querer", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2017], "xaxis": "x", "y": [35.66], "yaxis": "y"}, {"hovertemplate": "Novela=Renascer<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Renascer", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Renascer", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1993], "xaxis": "x", "y": [60.89], "yaxis": "y"}, {"hovertemplate": "Novela=O Rei do Gado<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Rei do Gado", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "O Rei do Gado", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1997], "xaxis": "x", "y": [51.81], "yaxis": "y"}, {"hovertemplate": "Novela=Terra Nostra<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Terra Nostra", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "Terra Nostra", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2000], "xaxis": "x", "y": [43.72], "yaxis": "y"}, {"hovertemplate": "Novela=Esperan\u00e7a<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Esperan\u00e7a", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Esperan\u00e7a", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2003], "xaxis": "x", "y": [37.95], "yaxis": "y"}, {"hovertemplate": "Novela=Velho Chico<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Velho Chico", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "Velho Chico", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2016], "xaxis": "x", "y": [29.05], "yaxis": "y"}, {"hovertemplate": "Novela=A Favorita<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A Favorita", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "A Favorita", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2009], "xaxis": "x", "y": [39.43], "yaxis": "y"}, {"hovertemplate": "Novela=Avenida Brasil<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Avenida Brasil", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "Avenida Brasil", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2012], "xaxis": "x", "y": [38.71], "yaxis": "y"}, {"hovertemplate": "Novela=A Regra do Jogo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A Regra do Jogo", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "A Regra do Jogo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2016], "xaxis": "x", "y": [28.54], "yaxis": "y"}, {"hovertemplate": "Novela=Segundo Sol<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Segundo Sol", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "Segundo Sol", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2018], "xaxis": "x", "y": [33.36], "yaxis": "y"}, {"hovertemplate": "Novela=Cavalo de A\u00e7o<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Cavalo de A\u00e7o", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "Cavalo de A\u00e7o", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1973], "xaxis": "x", "y": [50.5], "yaxis": "y"}, {"hovertemplate": "Novela=Roque Santeiro<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Roque Santeiro", "marker": {"color": "#FF97FF", "symbol": "circle"}, "mode": "markers", "name": "Roque Santeiro", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1986], "xaxis": "x", "y": [62.3], "yaxis": "y"}, {"hovertemplate": "Novela=Mandala<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Mandala", "marker": {"color": "#FECB52", "symbol": "circle"}, "mode": "markers", "name": "Mandala", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1988], "xaxis": "x", "y": [55.69], "yaxis": "y"}, {"hovertemplate": "Novela=O Fim do Mundo<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Fim do Mundo", "marker": {"color": "#636efa", "symbol": "circle"}, "mode": "markers", "name": "O Fim do Mundo", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1996], "xaxis": "x", "y": [46.81], "yaxis": "y"}, {"hovertemplate": "Novela=Amor \u00e0 Vida<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Amor \u00e0 Vida", "marker": {"color": "#EF553B", "symbol": "circle"}, "mode": "markers", "name": "Amor \u00e0 Vida", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2014], "xaxis": "x", "y": [35.51], "yaxis": "y"}, {"hovertemplate": "Novela=O Outro Lado do Para\u00edso<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Outro Lado do Para\u00edso", "marker": {"color": "#00cc96", "symbol": "circle"}, "mode": "markers", "name": "O Outro Lado do Para\u00edso", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2018], "xaxis": "x", "y": [38.23], "yaxis": "y"}, {"hovertemplate": "Novela=A Lei do Amor<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A Lei do Amor", "marker": {"color": "#ab63fa", "symbol": "circle"}, "mode": "markers", "name": "A Lei do Amor", "orientation": "v", "showlegend": true, "type": "scatter", "x": [2017], "xaxis": "x", "y": [27.22], "yaxis": "y"}, {"hovertemplate": "Novela=A Sombra da Rebecca<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "A Sombra da Rebecca", "marker": {"color": "#FFA15A", "symbol": "circle"}, "mode": "markers", "name": "A Sombra da Rebecca", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1967], "xaxis": "x", "y": [8.0], "yaxis": "y"}, {"hovertemplate": "Novela=Anast\u00e1cia, a Mulher sem Destino<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "Anast\u00e1cia, a Mulher sem Destino", "marker": {"color": "#19d3f3", "symbol": "circle"}, "mode": "markers", "name": "Anast\u00e1cia, a Mulher sem Destino", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1967], "xaxis": "x", "y": [5.47], "yaxis": "y"}, {"hovertemplate": "Novela=O \u00c9brio<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O \u00c9brio", "marker": {"color": "#FF6692", "symbol": "circle"}, "mode": "markers", "name": "O \u00c9brio", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1966], "xaxis": "x", "y": [5.23], "yaxis": "y"}, {"hovertemplate": "Novela=O Rei dos Ciganos<br>Ano=%{x}<br>Audiencia=%{y}<extra></extra>", "legendgroup": "O Rei dos Ciganos", "marker": {"color": "#B6E880", "symbol": "circle"}, "mode": "markers", "name": "O Rei dos Ciganos", "orientation": "v", "showlegend": true, "type": "scatter", "x": [1967], "xaxis": "x", "y": [6.7], "yaxis": "y"}],
                        {"legend": {"title": {"text": "Novela"}, "tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}, "xaxis": {"anchor": "y", "domain": [0.0, 1.0], "title": {"text": "Ano"}}, "yaxis": {"anchor": "x", "domain": [0.0, 1.0], "title": {"text": "Audiencia"}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('3450d477-8d7e-43f9-b0b6-9e63f62daf2b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
fig.write_html("audiencia-ano.html")
```

**Agora, a cereja do bolo!**

O Plotly oferece ferramentas para se plotar gráficos lindos em 3D. Ótimo para valorizar suas análises.


```python
fig = px.scatter_3d(df, x='Ano', y='Novela', z='Audiencia', size='Audiencia', color='Audiencia',
                    hover_data=['Novela'])
fig.update_layout(scene_zaxis_type="log")
fig.show()
```


<div>


            <div id="86305ab7-5bfd-451f-8f67-11a69c682c57" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};

                if (document.getElementById("86305ab7-5bfd-451f-8f67-11a69c682c57")) {
                    Plotly.newPlot(
                        '86305ab7-5bfd-451f-8f67-11a69c682c57',
                        [{"customdata": [["Champagne"], ["Meu Bem, Meu Mal"], ["Escalada"], ["O Casar\u00e3o"], ["Espelho M\u00e1gico"], ["Os Gigantes"], ["Roda de Fogo"], ["O Salvador da P\u00e1tria"], ["Selva de Pedra"], ["Rainha da Sucata"], ["A Pr\u00f3xima V\u00edtima"], ["Torre de Babel"], ["Bel\u00edssima"], ["Passione"], ["Partido Alto"], ["O Outro"], ["Tieta"], ["Pedra sobre Pedra"], ["Fera Ferida"], ["A Indomada"], ["Suave Veneno"], ["Porto dos Milagres"], ["Senhora do Destino"], ["Duas Caras"], ["Fina Estampa"], ["Imp\u00e9rio"], ["O S\u00e9timo Guardi\u00e3o"], ["Dancin\u2019 Days"], ["\u00c1gua Viva"], ["Brilhante"], ["Louco Amor"], ["Corpo a Corpo"], ["Vale Tudo"], ["O Dono do Mundo"], ["P\u00e1tria Minha"], ["Celebridade"], ["Para\u00edso Tropical"], ["Insensato Cora\u00e7\u00e3o"], ["Babil\u00f4nia"], ["Sangue e Areia"], ["Passo dos Ventos"], ["Rosa Rebelde"], ["V\u00e9u de Noiva"], ["Irm\u00e3os Coragem"], ["O Homem que Deve Morrer"], ["Selva de Pedra"], ["O Semideus"], ["Fogo sobre Terra"], ["Pecado Capital"], ["Duas Vidas"], ["O Astro"], ["Pai Her\u00f3i"], ["Cora\u00e7\u00e3o Alado"], ["S\u00e9timo Sentido"], ["Baila Comigo"], ["Sol de Ver\u00e3o"], ["Por Amor"], ["La\u00e7os de Fam\u00edlia"], ["Mulheres Apaixonadas"], ["P\u00e1ginas da Vida"], ["Viver a Vida"], ["Em Fam\u00edlia"], ["De Corpo e Alma"], ["Explode Cora\u00e7\u00e3o"], ["O Clone"], ["Am\u00e9rica"], ["Caminho das \u00cdndias"], ["Salve Jorge"], ["A For\u00e7a do Querer"], ["Renascer"], ["O Rei do Gado"], ["Terra Nostra"], ["Esperan\u00e7a"], ["Velho Chico"], ["A Favorita"], ["Avenida Brasil"], ["A Regra do Jogo"], ["Segundo Sol"], ["Cavalo de A\u00e7o"], ["Roque Santeiro"], ["Mandala"], ["O Fim do Mundo"], ["Amor \u00e0 Vida"], ["O Outro Lado do Para\u00edso"], ["A Lei do Amor"], ["A Sombra da Rebecca"], ["Anast\u00e1cia, a Mulher sem Destino"], ["O \u00c9brio"], ["O Rei dos Ciganos"]], "hovertemplate": "Ano=%{x}<br>Novela=%{customdata[0]}<br>Audiencia=%{marker.color}<extra></extra>", "legendgroup": "", "marker": {"color": [46.84, 50.34, 47.96, 51.82, 48.91, 49.29, 55.46, 62.05, 59.6, 60.91, 47.41, 43.56, 48.43, 35.13, 46.86, 57.01, 64.97, 54.39, 53.44, 47.85, 38.11, 44.36, 50.31, 41.04, 39.04, 32.73, 28.78, 60.0, 58.0, 44.64, 49.51, 52.67, 61.13, 43.31, 45.61, 46.05, 42.88, 35.78, 25.45, 10.74, 11.56, 21.96, 31.56, 48.2, 43.95, 48.68, 47.59, 52.15, 55.57, 57.44, 52.63, 61.6, 56.88, 45.96, 55.51, 44.8, 42.52, 44.9, 46.5, 47.08, 35.65, 29.65, 52.72, 47.37, 46.78, 49.21, 38.59, 33.97, 35.66, 60.89, 51.81, 43.72, 37.95, 29.05, 39.43, 38.71, 28.54, 33.36, 50.5, 62.3, 55.69, 46.81, 35.51, 38.23, 27.22, 8.0, 5.47, 5.23, 6.7], "coloraxis": "coloraxis", "size": [46.84, 50.34, 47.96, 51.82, 48.91, 49.29, 55.46, 62.05, 59.6, 60.91, 47.41, 43.56, 48.43, 35.13, 46.86, 57.01, 64.97, 54.39, 53.44, 47.85, 38.11, 44.36, 50.31, 41.04, 39.04, 32.73, 28.78, 60.0, 58.0, 44.64, 49.51, 52.67, 61.13, 43.31, 45.61, 46.05, 42.88, 35.78, 25.45, 10.74, 11.56, 21.96, 31.56, 48.2, 43.95, 48.68, 47.59, 52.15, 55.57, 57.44, 52.63, 61.6, 56.88, 45.96, 55.51, 44.8, 42.52, 44.9, 46.5, 47.08, 35.65, 29.65, 52.72, 47.37, 46.78, 49.21, 38.59, 33.97, 35.66, 60.89, 51.81, 43.72, 37.95, 29.05, 39.43, 38.71, 28.54, 33.36, 50.5, 62.3, 55.69, 46.81, 35.51, 38.23, 27.22, 8.0, 5.47, 5.23, 6.7], "sizemode": "area", "sizeref": 0.16242499999999999, "symbol": "circle"}, "mode": "markers", "name": "", "scene": "scene", "showlegend": false, "type": "scatter3d", "x": [1984, 1991, 1975, 1976, 1977, 1980, 1987, 1989, 1986, 1990, 1995, 1999, 2006, 2011, 1984, 1987, 1990, 1992, 1994, 1997, 1999, 2001, 2005, 2008, 2012, 2015, 2019, 1979, 1980, 1982, 1983, 1985, 1989, 1992, 1995, 2004, 2007, 2011, 2015, 1968, 1969, 1969, 1970, 1971, 1972, 1973, 1974, 1975, 1976, 1977, 1978, 1979, 1981, 1982, 1981, 1983, 1998, 2001, 2003, 2007, 2010, 2014, 1993, 1996, 2002, 2005, 2009, 2013, 2017, 1993, 1997, 2000, 2003, 2016, 2009, 2012, 2016, 2018, 1973, 1986, 1988, 1996, 2014, 2018, 2017, 1967, 1967, 1966, 1967], "y": ["Champagne", "Meu Bem, Meu Mal", "Escalada", "O Casar\u00e3o", "Espelho M\u00e1gico", "Os Gigantes", "Roda de Fogo", "O Salvador da P\u00e1tria", "Selva de Pedra", "Rainha da Sucata", "A Pr\u00f3xima V\u00edtima", "Torre de Babel", "Bel\u00edssima", "Passione", "Partido Alto", "O Outro", "Tieta", "Pedra sobre Pedra", "Fera Ferida", "A Indomada", "Suave Veneno", "Porto dos Milagres", "Senhora do Destino", "Duas Caras", "Fina Estampa", "Imp\u00e9rio", "O S\u00e9timo Guardi\u00e3o", "Dancin\u2019 Days", "\u00c1gua Viva", "Brilhante", "Louco Amor", "Corpo a Corpo", "Vale Tudo", "O Dono do Mundo", "P\u00e1tria Minha", "Celebridade", "Para\u00edso Tropical", "Insensato Cora\u00e7\u00e3o", "Babil\u00f4nia", "Sangue e Areia", "Passo dos Ventos", "Rosa Rebelde", "V\u00e9u de Noiva", "Irm\u00e3os Coragem", "O Homem que Deve Morrer", "Selva de Pedra", "O Semideus", "Fogo sobre Terra", "Pecado Capital", "Duas Vidas", "O Astro", "Pai Her\u00f3i", "Cora\u00e7\u00e3o Alado", "S\u00e9timo Sentido", "Baila Comigo", "Sol de Ver\u00e3o", "Por Amor", "La\u00e7os de Fam\u00edlia", "Mulheres Apaixonadas", "P\u00e1ginas da Vida", "Viver a Vida", "Em Fam\u00edlia", "De Corpo e Alma", "Explode Cora\u00e7\u00e3o", "O Clone", "Am\u00e9rica", "Caminho das \u00cdndias", "Salve Jorge", "A For\u00e7a do Querer", "Renascer", "O Rei do Gado", "Terra Nostra", "Esperan\u00e7a", "Velho Chico", "A Favorita", "Avenida Brasil", "A Regra do Jogo", "Segundo Sol", "Cavalo de A\u00e7o", "Roque Santeiro", "Mandala", "O Fim do Mundo", "Amor \u00e0 Vida", "O Outro Lado do Para\u00edso", "A Lei do Amor", "A Sombra da Rebecca", "Anast\u00e1cia, a Mulher sem Destino", "O \u00c9brio", "O Rei dos Ciganos"], "z": [46.84, 50.34, 47.96, 51.82, 48.91, 49.29, 55.46, 62.05, 59.6, 60.91, 47.41, 43.56, 48.43, 35.13, 46.86, 57.01, 64.97, 54.39, 53.44, 47.85, 38.11, 44.36, 50.31, 41.04, 39.04, 32.73, 28.78, 60.0, 58.0, 44.64, 49.51, 52.67, 61.13, 43.31, 45.61, 46.05, 42.88, 35.78, 25.45, 10.74, 11.56, 21.96, 31.56, 48.2, 43.95, 48.68, 47.59, 52.15, 55.57, 57.44, 52.63, 61.6, 56.88, 45.96, 55.51, 44.8, 42.52, 44.9, 46.5, 47.08, 35.65, 29.65, 52.72, 47.37, 46.78, 49.21, 38.59, 33.97, 35.66, 60.89, 51.81, 43.72, 37.95, 29.05, 39.43, 38.71, 28.54, 33.36, 50.5, 62.3, 55.69, 46.81, 35.51, 38.23, 27.22, 8.0, 5.47, 5.23, 6.7]}],
                        {"coloraxis": {"colorbar": {"title": {"text": "Audiencia"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "legend": {"itemsizing": "constant", "tracegroupgap": 0}, "margin": {"t": 60}, "scene": {"domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}, "xaxis": {"title": {"text": "Ano"}}, "yaxis": {"title": {"text": "Novela"}}, "zaxis": {"title": {"text": "Audiencia"}, "type": "log"}}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}},
                        {"responsive": true}
                    ).then(function(){

var gd = document.getElementById('86305ab7-5bfd-451f-8f67-11a69c682c57');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
fig.write_html("audiencia-novela-ano.html")
```

**Nosso objetivo com este estudo não é validar a dominância da maior emissora de televisão do país.    
Mas mostar visualizações incríveis podem auxiliar na contação de histórias de dados que inicialmente parecem tão sem forma e até mesmo vazios.**

Reparou como nos últimos 10 anos, a audiência das novelas vem caindo? Isso nos faz refletir sobre uma série de hipóteses. Sendo elas:

    1- Será que o Público vem perdendo o interesse em assistir novelas com o passar dos anos? 
    2- Será que com o advento da internet, o público apenas mudou de plataforma, deixando a TV para acompanhar suas sérias no Netflix?
    3- Ou será que de fato a concorrência tem assumido um papel relevante nessa batalha pelas telinhas de casa?
    4- Será que a Rede Globo está perdendo sua hegemonia? 
    
A resposta para estas perguntas, por enquanto, ficam para um próximo episódio. rsrsrs

![Bonner](https://media.giphy.com/media/H3GA54toeCMYUjaKAT/giphy.gif)
## Boa noite!

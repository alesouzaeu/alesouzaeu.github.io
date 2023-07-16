---
layout: post
title: Clusterização e Geolocalização com Foursquare.
image: pexels-sergio-souza-5047706.jpg
date: 2021-03-05 17:00:00 +0200
tags: coursera capstone sao_paulo brazil DataScience GeoMarketing Python GeoPy Pandas WebScraping
categories: geral
permalink: /category/geral/clusterizacao-e-geolocalizacao-com-foursquare/
---

Final project of the Coursera IBM Data Science Course

---
![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Brasão_da_cidade_de_São_Paulo.png)

# 1. Introduction: Business Problem

In this project we will try to find another ways to make a new marketing campains by a segmentation method. This report will be targeted to people interested in suggest marketing contents, produce advertising and etc,  to the people tha lives in the Districts of São Paulo's Capital, Brazil.

All entrepreneurs looks a way for increase the selling. Therefore, diferent marketing strategies are made of. 
We are talking about keeping a good presence in the social medias, to optimize the mechanisms of search, investing in custumer service. All of those are so good actions and also bring great results, but We are able to make more. 

We know that are lots of venues in the São Paulo City, therefore We will try figure out the business profile of each borough. Assuming that São Paulo is an alpha global city, We must to know what are the subject most reached by our costumers.

And finally, help them to make better buying decisions. We will use all power of data science to get a success we need.

# Districts of São Paulo City
We are talking about 96 Districts

![Alt ou título da imagem](https://miro.medium.com/max/1000/1*-tfHBjz7w73-YsBuM0JndA.png)
https://bit.ly/3bTma3a

------   

# 2.Data

The Data that We will use has been found at Wikipedia.com.br, and It exists since 2010.

So We must make a Web Scraping from page following this steps bellow.

   1. Grabbing the HTML Content from the URL
   2. Parsing the HTML content - BeautifulSoup
   3. Extructuring the content into a Data Frame - Pandas
   4. Converting and saving the Data like a JSON Archieve
   5. Importing that in our Jupyter Notebook
   
------   

#### How?
   
For that We will use some libs in Python language.

Like that:

        import time
        import requests
        import pandas as pd 
        from bs4 import BeautifulSoup
        from selenium import webdriver
        from selenium.webdriver.firefox.options import Options
        from selenium.webdriver.common.by import By
        import json

URL da Base de dados do Wikipedia: https://bit.ly/3vwSNvp

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset1.jpg)

------   

# 3. Methodology

This survey is a Clustering problem. In this case We will use a machine learning method.

**K- Means machine learning method** will be used for acquiring information concerning the venues located close to each area the data platform Foursquare was used. In this project our purpose is to create an appropriate number of clusters based on the Geolocalization and the quantity and the kind of venues located in each District in São Paulo City. 

In the end of this project we will be able to see the different types of public that we may consider important according to the kind of business We have. Also, We will figure out what kind of destinations each costumer has an interest. 
In general we acquire a clean perception of the most visitable venues in each District. From that We Will see what kind of venues we may offer like recommendations to each client.

------

#   4. Preparing Data

**4.1** Manipulation the dataframe to discover the coordinates of the  districts. 
For that we must make a tratament of our Dataset. 

The first step is to eliminate the NaN datas.
The second one consists on create a new column with the Address. 
(This is so important to get the coordinates of each Distric of São Paulo)

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset2.jpg)

The Coordinates had been gotten by The Library GeoPy, that fetchs the Latitude and Longitude from the Address, how We said lastly.


**from geopy.geocoders import Nominatim**


![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset3.jpg)

------   

# 5. Visualizing Data

**5.1** Now We will utilize the Foursquare API to figure out the Latitude and Longitute. 
The next step is to plot the Maps through the Folium Library

![Alt ou título da imagem](data:image/png;base64,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)

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/map1.jpg)

How We can see, each blue point represents each District of our dataset. But It isn't enough. 
We will need to figure out all kind of Venues we can find from each reference points.

**5.2** It is awesome thinking the Foursquare API is able to return each name and each category of the business places.

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset4.jpg)

**5.3** We are talking about 100 places returned by Foursquare API. There are 290 uniques categories.

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset5.jpg)

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset5.jpg)

**5.4** How we know, To work with Machine Learning, It is necessary to convert the categorical variables into numerical variables.

For that we have used The One Hot Encoding method.

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset7.jpg)

**5.5** In case of this business problem We want to discover the most common Venue of São Paulo's District. All of this driven by location.

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset9.jpg)

------   

# 6. Modeling

**6.1** It's interesting We figure out the Best K number for a Clustering Algorithm. In this case, We are using K-Means algorithm and the number of K represents the best quantities of Clusters may use for.

So, We might have used the Elbow Method. This Approach is very common used with the K-means Clustering. So We have got that K is equal 6.

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/k-plot1.jpg)


```python
# set number of clusters
kclusters = 6

sp_grouped_clustering = sp_grouped.drop('Neighborhood', 1)

# run k-means clustering
kmeans = KMeans(n_clusters=kclusters, random_state=0).fit(sp_grouped_clustering)

# check cluster labels generated for each row in the dataframe
kmeans.labels_[0:10] 
```

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/dataset10.jpg)

**6.2** After merging the data, We had needed to make a new tratament of the dataset. 

**6.3** Now We have a new map with the clusters separated by colors. It's simply wonderful! 
The 6 clusterings separated by Colors! 

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/map2.jpg)

# The Clustering consists in:

## Cluster 1

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Cluster1.jpg)

## Cluster 2

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Cluster2.jpg)

## Cluster 3

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Cluster3.jpg)

## Cluster 4

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Cluster4.jpg)

## Cluster 5

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Cluster5.jpg)

## Cluster 6

![Alt ou título da imagem](https://raw.githubusercontent.com/alesouzaeu/Coursera_Capstone/main/The_Battle_of_the_Neighborhoods/images/Cluster6.jpg)

------   

# 7. Conclusion

**7.1** Finally We can see the different categories of the Venues in our dataset. 
So, How can this study help us? 

With those tools We will able to make recommendations to each public in each clustering. We may use the neighborhood, distance, the position in the map, the various combination of informations to suggest the best product or service to our clients. 

It is a good way to construct a New marketing campain to your constumers. Dada help us to understand our public and get less mistakes than other market players.


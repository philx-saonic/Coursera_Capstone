# Ethnic Diversity in Houston and Dallas

## 1. Introduction

Houston, the fourth biggest city in the United States and the biggest city in Texas, is known for its ethnic diversity. Dallas, also a big city in Texas, is one of the fastest-growing cities in the past few years, and many people have moved to the city from other states or even other countries. Although Dallas' population is not as large as Houston's, the city is becoming more diverse in ethnicity. <br/>

This project aims to understand how similar Dallas and Houston are in terms of ethnic diversity. This information will be valuable to people who are interested in opening business for some specific ethnic groups. 


## 2. Data Acquisition and Cleaning

### 2.1 Data Sources
In this project, data of postal codes of Houston and Dallas areas and their respective latitudes and longitudes are needed. The postal code data are obtained from [World Postal Code](https://worldpostalcode.com/united-states/texas/), and the data of latitudes and longitudes are acquired through the Geocoding API from Google Cloud Platform.
This project also collected the data of the popular places in those postal code areas through Foursquare API.

### 2.2 Data Cleaning
After importing CSV files of all the postal codes of the two cities, which were scraped from the website, this project obtained the latitudes and longitudes of each postal code area via Geocoding API. Then, I obtained the data of all the venues around those postal code areas, given latitudes and longitudes as locators. The data collected include the venues' name, coordinates, and categories. I stored the data of venues in Houston and Dallas in separate data frames, which have 13513 rows × 7 columns and 11583 rows × 7 columns respectively. 

Next, this project turned the categorical data into dummy variables (one-hot encoding). Then, the data with dummy variables were grouped by postal codes and the mean of the frequency of occurrence of each category was extracted into another data frame. Now, the data presenting the frequency of occurrence of each category in each postal code area are obtained. The frequency of occurrence is in the range of [0, 1]. Here is the example of the data described above: 
![alt text](https://imgur.com/bPtndye.png)

The frequency of occurrence above is used for getting the top 10 most common venue categories in those areas. This project assumes the top 10 popular venues are enough to represent business types of an area. Here is the example of the result data:
![alt text](https://imgur.com/JFfJTfQ.png)



The result data frame of Houston has 180 rows and Dallas' has 139 rows, which are equal to the numbers of postal codes in Houston and Dallas.

## 3. Exploratory Data Analysis
### 3.1 Value Counts of Variables
This project uses `pandas_profiling` package to create profile reports, included in the jupyter notebook of this project. The reports the value counts of variables in the data; however, it is a very long list, so only the analysis and discussion will be included in this report.

Because the data are all categorical, the analysis will focus on the Variable part of the information.

For Houston, fast food and Mexican restaurants are the most popular among different types of restaurants. Since only the data of the top 10 venues in each postal code area are collected, all venues appear in the summary can be considered popular. From the value counts of multiple columns in the data, it can be known that there are many other types of restaurants in Houston: Middle Eastern, Vietnamese, Italian, and Chinese. 

Similar to Houston, Dallas also has a lot of Mexican restaurants, fast food restaurants, pizza places, etc. But other types of cuisine such as Vietnamese and Chinese seem to appear less in the summary than Houston.

It is possible that some other popular venues were not shown in that brief summary analysis. Thus, I will use word clouds to visualize the frequency of the venues.

### 3.2 Word Clouds
The data frame examples above show that there are many words like "Shop", "Restaurant", and "Place" exist in the data. Those are not the words which this project is interested in. Instead, this project is looking for words such as "Mexican" and "Chinese" that can provide ethnic information. Therefore, terms like "Shop", "Restaurant", and "Food" had been added to the default stop words set before the word clouds were generated.
The word clouds of the two cities are shown below.

#### Word Clouds of Houston
![alt text](https://imgur.com/lVwxseo.png)

#### Word Clouds of Dallas
![alt text](https://imgur.com/mHUELof.png)

Obviously, the most popular restaurants in Houston and Dallas are very similar; Mexican, pizza, fast food, and sandwich places are very popular in two cities. Taking a closer look, we can find that the word "Vietnamese" in Houston's word clouds seems to be a little bigger than the one in Dallas', which may indicate that the ratio of Vietnamese restaurants in Houston is higher. 


## 4. Clustering
This project also seeks to understand the areas in both cities better through clustering these areas based on their types of venues.

#### Clusters of Houston
The unsupervised learning **K-Means Algorithm** is used to cluster the areas in the two cities. The K for the algorithm is manually selected. For Houston, this project used `k=6` to run K-Means Algorithm.

![alt text](https://imgur.com/QqzzskB.png)

* **Houston Cluster 1 (Red, 22 rows): "Hotel & Bar Venues"** <br/>
Hotels, bars (and cocktail bars), parks and burger joints are the most popular venues in this cluster, which represent the red points in the map. Mexican, Italian, and Vietnamese restaurants are also popular in this area.

* **Houston Cluster 2 (Purple, 22 rows): "Daily Life Venues"** <br/>
Fast food restaurants, fried chicken joints, pizza places, discount stores, grocery stores, and gas stations are the top venues of this cluster. These points seem to spread more equally in the map, which may explain the top types of venues of this cluster, since those venues are the kinds of places that people might visit very often, and they are demanded in almost every area.

* **Houston Cluster 3 (Blue, 1 row): Outlier 1** <br/>
This cluster only has one row, which can be regarded as an outlier.

* **Houston Cluster 4 (Light Blue-Green, 70 rows): "Mexican and Fast Food Venues"** <br/>
Mexican restaurants and fast food restaurants are the most popular venues in this cluster. Some other kinds of venues in everyday demand such as gas stations, grocery stores, and discount stores are also common in this cluster.

* **Houston Cluster 5 (Green, 64 rows): "Mixed Venues"** <br/>
There are many different kinds of restaurants in this cluster; Mexican and Vietnamese restaurants are still popular, but when compared to other clusters, this cluster obviously has more middle eastern and Mediterranean restaurants. We can also see Indian, Chinese, Italian, Cajun/Creole, Creperie, and Japanese restaurants appear many times in this cluster.

* **Houston Cluster 6 (Orange, 1 row): Outlier 2** <br/>
Since it only has one row, we can consider it an outlier.

#### Clusters of Dallas
For Dallas, this project used `k=5` to run K-Means Algorithm.

![alt text](https://imgur.com/WbuCxvv.png)

* **Dallas Cluster 1 (Red, 32 rows): "Random venues"** <br/>
There are many kinds of venues and it is hard to tell which types of venues are more popular than others.

* **Dallas Cluster 2 (Purple, 51 rows): "Hotel, Bar & Coffee Venues"** <br/>
Purple points are very concentrated in some areas; this makes their areas within a radius of 2500 meters highly overlapped. Thus, many postal code areas have identical top 10 venues. Although cluster 2 only represents some small areas, we can still see that the most popular venues in this cluster are hotels, bars, and coffee shops.

* **Dallas Cluster 3 (Blue, 50 rows): "Generally Popular Venues"** <br/>
This cluster seems to have many generally popular places such as Mexican and fast food restaurants, pizza place, convenience and discount stores. One thing interesting is that in the area of 75378, the top 1 venue is Korean restaurant, which does not appear often in the data. That area may be the Korean town of Dallas.

* **Dallas Cluster 4 (Green, 50 rows): "Mexican & Taco Venues"** <br/>
Mexican restaurants and Taco places are very popular in this cluster. There are some French restaurants in this cluster, which are rare in other clusters of Dallas.

* **Dallas Cluster 5 (Orange, 1 row): Outlier** <br/>
This cluster only has one row, so it can be regarded as an outlier.

## 5. Discussion & Result
From the word clouds, it can be seen that the two Texas cities have some similar characters such as the popularity of Mexican, fast food, pizza restaurants, and sandwich places. The latter three are very popular all over the United States, but Mexican food is a special feature of Texas.

In the clustering part, the clusters show several groups of venues in each city. It helps to gain more understanding of what kinds of venues are more likely to be closer to some specific places and where they are located at. However, it is difficult to draw a clear conclusion of how different Houston and Dallas are in the aspect of ethnic groups from those clusters because they are similar in many ways. But through observing the cluster, it can be told that Houston is a more diverse city since there is a "Mixed Venues" cluster in Houston and it includes many types of restaurants that rarely seen in Dallas, such as Cajun/Creole and Creperie. The number of restaurants of different cultures such as Chinese, Japanese, and Italian in Houston is larger than Dallas' as well.

## 6. Conclusion
To sum up, this project analyzed the types of venues in the Houston and Dallas area and sought to draw a conclusion of how different Houston and Dallas are in the aspect of ethnic groups. Nevertheless, through the analysis, this project was only able to find that Houston is a more diverse city than Dallas in terms of more restaurants or places of different cultures. More data might be needed to suggest business opportunities and their locations.


## References
1. [World Postal Code](https://worldpostalcode.com/united-states/texas/)
2. [Foursquare API] (https://developer.foursquare.com/)
3. [Google Map](https://www.google.com/maps/)
4. [Google Geo-location API](https://cloud.google.com/maps-platform/)
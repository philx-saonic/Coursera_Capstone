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
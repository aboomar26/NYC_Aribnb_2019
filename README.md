New York City Airbnb Data Analysis
Project Overview
This project explores the Airbnb market in New York City using a dataset from 2019. The analysis aims to uncover trends, patterns, and insights that can help hosts and potential investors make informed decisions. The project involves data cleaning, preparation, and extensive exploratory data analysis (EDA), culminating in visualizations and insights about the Airbnb market in NYC.

Dataset
The dataset used in this analysis is the "AB_NYC_2019.csv" file, which contains detailed information about Airbnb listings in NYC, including prices, reviews, availability, and more.

Dataset Features
id: Unique identifier for each listing
name: Name of the listing
host_id: Unique identifier for each host
host_name: Name of the host
neighbourhood_group: Major borough (e.g., Manhattan, Brooklyn)
neighbourhood: Specific neighborhood within the borough
latitude: Latitude of the listing
longitude: Longitude of the listing
room_type: Type of room (e.g., Entire home/apt, Private room)
price: Price per night in USD
minimum_nights: Minimum number of nights required to book
number_of_reviews: Total number of reviews
last_review: Date of the last review
reviews_per_month: Number of reviews per month
calculated_host_listings_count: Number of listings per host
availability_365: Number of days the listing is available in a year
Getting Started
Prerequisites
Python 3.x
Jupyter Notebook
Pandas
NumPy
Matplotlib
Seaborn
Installation

File Structure
AB_NYC_2019.csv: The dataset file.
NYC-XX-985481.jpeg: A map of New York City used for geospatial visualization.
analysis.ipynb: The Jupyter notebook containing the data analysis and visualizations.
README.md: This README file.
Data Cleaning and Preparatio

Data Cleaning and Preparation
Loading the Data:

import pandas as pd

df = pd.read_csv("AB_NYC_2019.csv", encoding='unicode_escape')

Initial Exploration:

Check the shape of the data:


df.shape
Display the first few rows:


df.head()
Get information about data types and null values:


df.info()
df.isnull().sum()
Handling Missing Values:

Fill missing values in reviews_per_month with 0:


df['reviews_per_month'].fillna(0, inplace=True)
Convert last_review to datetime and fill missing values with "Never":


df['last_review'] = pd.to_datetime(df['last_review'].str.strip())
df['last_review'].fillna("Never", inplace=True)
Dropping Irrelevant Columns:



df.drop(columns=['id', 'name', 'host_name'], inplace=True)
Removing Outliers:

Remove listings with minimum_nights greater than 365:


df = df[df['minimum_nights'] <= 365]
Data Engineering
Categorized Minimum Nights:



min_night_type = []
for i in range(len(df)):
    if df['minimum_nights'].iloc[i] <= 5:
        min_night_type.append('1-5 days')
    elif df['minimum_nights'].iloc[i] <= 14:
        min_night_type.append('1-2 weeks')
    elif df['minimum_nights'].iloc[i] <= 90:
        min_night_type.append('1-3 months')
    elif df['minimum_nights'].iloc[i] <= 180:
        min_night_type.append('4-6 months')
    else:
        min_night_type.append('7-12 months')

df['min_night_type'] = min_night_type
Calculated Host Listings Count:



df['calculated_host_listings_count'] = df.groupby('host_id')['host_id'].transform('count')
Exploratory Data Analysis (EDA)
Univariate Analysis
Neighborhood Distribution:



import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
sns.countplot(x='neighbourhood_group', data=df)
plt.title('Distribution of Listings by Neighborhood Group')
plt.show()
Room Type Distribution:



plt.figure(figsize=(10, 6))
sns.countplot(x='room_type', data=df)
plt.title('Distribution of Listings by Room Type')
plt.show()
Price Distribution:



plt.figure(figsize=(10, 6))
sns.histplot(df['price'], bins=50, kde=True)
plt.title('Price Distribution')
plt.show()
Bivariate Analysis
Price vs. Reviews:



plt.figure(figsize=(10, 6))
sns.scatterplot(x='number_of_reviews', y='price', data=df)
plt.title('Price vs. Number of Reviews')
plt.show()
Boxplots of Prices by Neighborhood Group:



plt.figure(figsize=(10, 6))
sns.boxplot(x='neighbourhood_group', y='price', data=df)
plt.title('Price Distribution by Neighborhood Group')
plt.show()
Boxplots of Number of Reviews by Neighborhood Group:

  
plt.figure(figsize=(10, 6))
sns.boxplot(x='neighbourhood_group', y='number_of_reviews', data=df)
plt.title('Number of Reviews by Neighborhood Group')
plt.show()
Geospatial Analysis
Scatter Plot on NYC Map:


nyc_map = plt.imread("NYC-XX-985481.jpeg")
plt.figure(figsize=(10, 20))
plt.imshow(nyc_map, zorder=1, extent=[-74.31, -73.66, 40.49, 40.91])
sns.scatterplot(y='latitude', x='longitude', data=df, hue='room_type', size='price', style='room_type', sizes=(50, 200))
plt.show()
Key Insights
Neighborhood Popularity:

Manhattan and Brooklyn have the highest number of Airbnb listings.
Queens, Bronx, and Staten Island offer unique opportunities with fewer listings but potential for growth.
Room Types:

Entire homes/apartments are the most common listings, followed by private rooms and shared rooms.
Price distributions vary significantly between room types.
Pricing Strategies:

Detailed analysis of price distribution across different neighborhoods and room types reveals varying pricing strategies.
Higher prices are generally found in Manhattan, with more affordable options in the Bronx and Staten Island.
Host Analysis:

Identification of top hosts by the number of listings.
Analysis of host strategies and the impact of having multiple listings.
Review Patterns:

Insights into the distribution of reviews and their impact on pricing.
Listings with more reviews tend to have higher prices, indicating popularity and demand.
Future Work
Trend Analysis:

Integrate more recent data to analyze trends over time and understand market dynamics.
Machine Learning Models:

Apply machine learning models for price prediction and demand forecasting.
Explore clustering algorithms to identify similar neighborhoods and optimize pricing strategies.
Impact of Regulations:

Investigate the impact of new regulations on the Airbnb market in NYC.
Analyze changes in listings, prices, and availability post-regulation implementation.
Conclusion
This analysis provides a comprehensive overview of the NYC Airbnb market, offering valuable insights for hosts and investors. The project demonstrates the power of data science in uncovering trends and patterns in real-world datasets.

Contact
For any questions or collaborations, feel free to reach out:

LinkedIn: [Your LinkedIn Profile]
Email: [Your Email]
Acknowledgments
The dataset is sourced from Inside Airbnb.



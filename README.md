# Zomato Bengaluru Restaurant Analytics & Recommendation System

## Overview

This repository presents an end-to-end data analytics and business intelligence project built on a real-world dataset from Zomato, covering thousands of restaurants in Bengaluru. The project includes thorough data cleaning, feature engineering, geospatial mapping, sentiment analysis from customer reviews, and the development of two fully interactive Power BI dashboards. It is designed as a practicum project for the MSc in Analytics program at NTU, with a focus on urban food intelligence, service accessibility, and data-driven recommendation systems.

## Project Goals

#### 1. Clean and preprocess the Zomato dataset containing restaurant metadata and user reviews.

#### 2. Perform Exploratory Data Analysis (EDA) to uncover trends in pricing, cuisine, ratings, and locations.

#### 3. Conduct geospatial analysis to visualize and understand spatial dynamics of restaurants in Bengaluru.

#### 4. Perform Natural Language Processing (NLP) to analyze customer reviews and derive sentiment-based scores.

#### 5. Engineer composite metrics such as weighted review scores combining sentiment and votes.

#### 6. Design Personalized recommendation dashboard for users.

## Data Description

### The raw dataset, collected from Zomato, includes ~51,000 restaurant records with the following key fields:

- Url, name, location, address, phone, votes, rating

- cuisines (multi-valued)

- rest_type, listed_in(type), listed_in(city)

- approx_cost(for two people)

- online_order, book_table

- reviews_list (as list of tuples)

- lat, lon (added during geocoding phase)


## 1. Data Cleaning and Feature Engineering

- Missing values in rate were imputed using linear regression, trained on votes and approx_cost.

- approx_cost(for two people) was cleaned and missing values filled with the mean.

- Categorical fields like rest_type, cuisines, and listed_in(type) were standardized.

- Phone numbers were cleaned using regex.

### New variables created:

- price_range: Bucketed cost bins (0-500, 500-1000, etc.)

- service_score: Binary sum of online_order and book_table, shows positive correlation between ratings and scores
  
 ![image](https://github.com/user-attachments/assets/0bdd1447-5bfa-45c1-b8d5-3eaeb5e89c3d)

- CuisineDim and ServiceTypeDim: Exploded dimension tables for many-to-one relationships (for efficient dashboard slicing)


## 2. Exploratory Data Analysis (EDA)

### Notable Visuals

- Votes vs Ratings: Positive correlation, especially for restaurants with booking options.
  
  ![image](https://github.com/user-attachments/assets/c0af26a8-c9b0-4048-94c8-d2b4468c62c6)

- Top Restaurants: Count plot showing multi-outlet chains like Cafe Coffee Day dominate.

  ![image](https://github.com/user-attachments/assets/0f178f80-0f66-4465-a21c-24b42c92f667)

- Location Wise Analysis: Central dining areas like MG Road, Church Street, Lavelle Road have higher ratings on average although with increased cost.
  
  ![image](https://github.com/user-attachments/assets/4cd9f2c8-415b-4cc9-b384-e4094e235db0)

- Cuisine Popularity: North Indian leads, followed by Chinese, South Indian, and Fast Food.

  ![image](https://github.com/user-attachments/assets/e22b52dd-21a6-4610-87f1-820891d5d5a2)


## 3. Geospatial Mapping with Folium 

### Maps Created (can be accessed in the EDA notebook)

- Restaurant HeatMap: Shows density clusters in the central and southeastern regions.
  
  ![image](https://github.com/user-attachments/assets/f0c0224e-be5f-4ff6-851d-52d2def0395f)

- Cuisine Marker Maps:
  
  - Top 5 cuisines: North Indian, South Indian, Chinese, Desserts, Fast Food.

  - Markers coded by rating (green = high, orange = mid, red = low).

  - Icon shape based on cuisine.

  - Highlights high-rated North Indian and Chinese food in Indiranagar, Koramangala, MG Road.
    
  - Map access link: https://shivang-18.github.io/top5_cuisines_by_location.html
    
  ![image](https://github.com/user-attachments/assets/b635059f-64c6-46a2-98bf-814ad3a3f723)


- Price Range Map:

  - Markers represent modal price range per locality.

  - Central areas show higher spending brackets (3500+ INR), suburbs show budget-friendly options.
    
  ![image](https://github.com/user-attachments/assets/691ce8bd-8a08-4cb3-adba-8d8b0a73a0c4)


## 4. Sentiment Analysis from Reviews

- Input Format: reviews_list was a stringified list of tuples: ('Rated 4.0', 'This is the review...')

- Processing Steps:

  - Parsed using ast.literal_eval

  - Cleaned using ftfy, regex, lowercase conversion, punctuation removal

  - Sentiment polarity extracted using TextBlob (score: -1 to +1)

- Aggregation:

  - Per restaurant: computed avg sentiment, avg extracted rating, and review count

  - Weighted with log(votes) to prioritize trusted reviews

  - Scaled using MinMaxScaler to 0–5: scaled_review_score

### Visual Analysis of derived scores

- Boxplot: Showed scaled sentiment scores to be more conservative and diverse than official app ratings

  ![image](https://github.com/user-attachments/assets/ade68b55-8ffb-4cad-8fc1-a66c31d9d1d9)

- Regression Scatterplot: Positive correlation between app rating and scaled sentiment score (but with outliers)

  ![image](https://github.com/user-attachments/assets/662f4c74-38f4-498c-bfae-e370e8163122)

- Rating Difference Bar plot: Positive differences (mostly in residential areas) indicate that the platform’s raw star ratings tend to exceed the sentiment driven scores, suggesting a mild inflation of stars relative to the textual feedback. Negative ones (majorly in central city) reveal that customer reviews in these districts convey stronger positive sentiment than their star ratings alone would imply.

  ![image](https://github.com/user-attachments/assets/fae8b532-633f-4cb7-acd8-1dc42599b501)




## 5. Dashboard in Power BI

### Restaurant Recommendation Smart Dashboard

![image](https://github.com/user-attachments/assets/674ed160-b3f7-448c-b4d7-ecf7c72911b7)

- User Inputs: Budget, Cuisine, Booking, Cost, Rating, Location options

- Output:

  - Dynamic ranked restaurant list

  - Map markers and cuisine breakdown for selections


## Key Learnings & Results

- Combined structured and unstructured data to deliver actionable insights

- Created a sentiment-aware composite score more reliable than standalone app ratings

- Visualized pricing and quality clusters across Bengaluru

- Delivered a modular, extensible framework for urban food intelligence


## Getting Started

- Clone the repo

- View Jupyter notebooks in /notebooks

- Open .pbix files in Power BI Desktop (Free)


## Technologies Used

#### 1. Python: Pandas, NumPy, Seaborn, Matplotlib, TextBlob, FTFY, Folium

#### 2. Power BI (Dashboards)

#### 3. Jupyter Notebook

#### 4. Git/GitHub for version control

## Author
Shivang Singh

## License
This repository is made available for academic and non-commercial use. Please credit the author if you reference or build upon this work.

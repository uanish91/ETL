# Building ETL Data Pipeline using Python and SQLite
Imagine a giant library of news articles, all from different websites and countries. That's basically the News API! It lets programmers tap into this library and pull out news data to add to their creations, like apps or websites. This saves them time and makes everything nice and organized.

In this project, we are going to use news-api python library that provides large collection of News articles. You may think of it as google news where news from different sources appear at one place.
The project was performed in Google Colab notebook. The API key to connect with the web service was acquired from https://newsapi.org/register.

We will start the project by importing necessary libraries. Following libraries were imported. 
  a. pandas (For data loading and wrangling)
  b. sqlite3 (SQLite database management)
  c. NewsApiClient (A python client library)
  d. logging (For managing log messages)

# 1. Retrieve and print articles (Extract stage of ETL)
  - Define a function to extract data (extract_news_data()) from newsapi by setting the parameter 'q' to AI. This will help pull all the AI related articles in json format.
    # Define the functions to overlook the extracted data and clean it. 
  - There is an optional step where keys from key:value pair of the json file are obtained so we know what keys are important for our requirement.
  - The data from Author arrtibute was formatted to title case letter by defining method clean_author_column(text)

 # 2. Transform the retrieved data (Transform stage of ETL)
  - The exracted data was further transformed for staging in sqlite database. The step is performed by defining transform_news_data(articles).
  - For each dictionary in JOSN data, key values of author, title, publishedAt, content, and url are extracted.
  - The value for source key is replaced with value for name key within the source dictionary.
  - Load the newly transformed data into dataframe format using pandas and assign the respective columns.
  - Convert the Date Published column to Datetime format and apply the fucntion created to clean the Author column.

# 3. Load the Data into SQLite database.
  - Create a function to load data into SQLite database by using cursor object.
  - Create a table in the SQLite database system to load the pandas dataframe into the database.

# 4. Initialize the DAG object.
  - ETL process is now completed. But in order to continuously pull the data manually from the newapi, transform and load will take a lot of time.
  - Apache airflow is required to automate this process.
  - Initialize a DAG objects with parameters having current datetime and schedule to run the ETL cycle daily.

# 5. Use Xcoms to approach to make our defined functions compatible to Airflow. This helps is sharing data between tasks.
  - Change the extract_news_data() to accept KWARGS and push the data to Xcom. 
  - Change the transform_news_data() function to pull the data from Xcom and push back to Xcom after the formatting(cleaning)
  - Change the load_news_data() function to pull data from Xcom and and transform it to dataframe.

The ETL process is automated using apache airflow.

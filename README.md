Project Overview
This project involves web scraping IMDb for movie ratings, reviews, and related data, followed by data cleaning to prepare a high-quality dataset for analysis. The project was developed using Python and Jupyter Notebook.

Functionalities
Data Scraping: Scrapes IMDb's "Most Popular Movies" chart to extract movie details such as title, URL, description, ratings, genre, and duration.
Data Storage: Saves the scraped data to a CSV file for further use.
Data Cleaning: Processes the data to handle missing values, remove duplicates, and ensure a structured format.
Cleaned Dataset: Generates a cleaned dataset ready for exploration and analysis.

Prerequisites
Python 3.8+
Libraries:
requests
BeautifulSoup (from bs4)
pandas
json
Jupyter Notebook (recommended for running scripts)

Project Structure
project-directory/
├── most_popular_movies.csv       # Raw scraped data
├── cleaned_most_popular_movies.csv # Final cleaned dataset
├── scripts/
│   ├── data_scraping.py          # Script for data scraping
│   ├── data_cleaning.py          # Script for data cleaning
└── README.md                     # Project documentation

How to Use
1. Data Scraping
Run the following script to scrape data from IMDb and save it as a CSV file.

import requests
from bs4 import BeautifulSoup
import pandas as pd
import json

# URL for IMDb's Most Popular Movies
url = "https://www.imdb.com/chart/moviemeter/"
headers = {"User-Agent": "Chrome/131.0.6778.109"}
response = requests.get(url, headers=headers)

if response.status_code == 200:
    print("Successfully fetched the page!")
else:
    print(f"Failed to retrieve the page. Status code: {response.status_code}")

# Parse HTML
soup = BeautifulSoup(response.text, 'html.parser')

# Extract JSON-LD data
script_tag = soup.find('script', type='application/ld+json')
if script_tag:
    data = json.loads(script_tag.string)

# Extract data fields
titles, urls, descriptions, best_ratings, worst_ratings, rating_values, genres, durations = [], [], [], [], [], [], [], []
if 'itemListElement' in data:
    for item in data['itemListElement']:
        movie = item['item']
        titles.append(movie.get('name', 'N/A'))
        urls.append(movie.get('url', 'N/A'))
        descriptions.append(movie.get('description', 'N/A'))
        best_ratings.append(movie.get('aggregateRating', {}).get('bestRating', 'N/A'))
        worst_ratings.append(movie.get('aggregateRating', {}).get('worstRating', 'N/A'))
        rating_values.append(movie.get('aggregateRating', {}).get('ratingValue', 'N/A'))
        genres.append(movie.get('genre', 'N/A'))
        durations.append(movie.get('duration', 'N/A'))

# Save to CSV
df = pd.DataFrame({
    'Title': titles,
    'URL': urls,
    'Description': descriptions,
    'Best Rating': best_ratings,
    'Worst Rating': worst_ratings,
    'Rating Value': rating_values,
    'Genre': genres,
    'Duration': durations
})
df.to_csv('most_popular_movies.csv', index=False)
print("Data saved to 'most_popular_movies.csv'")

2. Data Cleaning
Run the following script to clean the scraped data.
import pandas as pd

# Load raw data
file_path = r"C:\Users\Aakriti K C\Desktop\DA and AI\project 1 IMDB\bda66-imdb-movie-reviews-scraping-cleaning-the-movie-ratings-bda66imdb-s526124\most_popular_movies.csv"
df = pd.read_csv(file_path)

# Inspect dataset
print("Dataset Info Before Cleaning:")
print(df.info())
print("\nSample Data:")
print(df.head())

# Clean data
df['Description'] = df['Description'].fillna("No description available")
df['Genre'] = df['Genre'].fillna("Unknown")
df.dropna(subset=['Title', 'Rating Value'], inplace=True)
df.drop_duplicates(inplace=True)

# Save cleaned data
cleaned_file_path = r"C:\Users\Aakriti K C\Desktop\DA and AI\project 1 IMDB\bda66-imdb-movie-reviews-scraping-cleaning-the-movie-ratings-bda66imdb-s526124\cleaned_most_popular_movies.csv"
df.to_csv(cleaned_file_path, index=False)
print(f"Cleaned data saved to {cleaned_file_path}")

3. Final Dataset
The final cleaned dataset is saved as cleaned_most_popular_movies.csv and is ready for analysis.


Sample Data
Here is a sample from the cleaned dataset:

Title	URL	Description	Rating Value	Genre	Duration
The Batman	https://imdb.com/title/tt1877830/	A new DC movie.	8.3	Action, Drama	PT2H55M
Dune	https://imdb.com/title/tt1160419/	Sci-fi epic.	8.1	Sci-Fi, Drama	PT2H35M

Features
Scraping:
Title
URL
Description
Best and worst ratings
Rating value
Genre
Duration

Cleaning:
Handles missing values.
Removes duplicates.
Ensures critical fields like Title and Rating Value are non-empty.

Usage Instructions
Ensure all required Python libraries are installed.
Run the scraping script to fetch raw data from IMDb.
Run the cleaning script to process and clean the data.
Use the cleaned dataset for analysis or visualization.

License
This project is for educational purposes. IMDb content belongs to IMDb.com.




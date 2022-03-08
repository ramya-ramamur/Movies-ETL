# Movies-ETL.

# Overview
The purpose is to create an automated pipeline that takes in new data, performs the appropriate transformations, and loads the data into existing tables. To do this, Movie data from 1990 to 2018 is analyzed from Wikipedia json file, extracted large data set from Kaggle, then transforming the data into a usable dataset for a "hacking competition." Once the data was transformed and narrowed in scope for the hack-a-thon, the DataFrames were loaded into PostgresSQL. 

### Process: ETL (Extract, Transform, Load)
The main goal is to create a refactorable and intuitive ETL Pipeline that helps automate processing large sets of data.

**Extract** - extract scraped Wikipedia data stored as a JSON, and Kaggle data stored in CSVs
  * Create an ETL pipeline from raw data to a SQL database.
  * Extract data from disparate sources using Python.
**Transform**   
  * Clean and transform data using Python, Pandas.
  * Use regular expressions to parse data and to transform text into numbers.
**Load** 
  * Load data with PostgreSQL.

# Resources
* Python 3.7.6, JupyterLab 2.26
* PostgreSQL 12.2, Pgadmin 4.20
* **Data Source**
   * Wikipedia: (format: json) 7,311 thousand movie titles that include information about the movies, including budgets, box office returns, from 1990 to 2018. 
   **wikipedia-movies.json** is in the Resources folder
   * Kaggle: - 2 files (format: .csv): [Movie Database download link.](https://www.kaggle.com/rounakbanik/the-movies-dataset/download) 
   MovieLens is a website run by the GroupLens research group at the University of Minnesota. The Kaggle dataset pulls from the MovieLens dataset of over 20 million reviews and contains a metadata file with details about the movies from The Movie Database (TMDb). Files we will be using are:
      - movies_metadata.csv (in Resource folder)
      - ratings.csv (note: due to size of the raw data files, they are not included within this repo. A sample with 1000 entries is included in the Resources folder. For the entire dataset, click on the "The Movie Database" link above).

# Results

## Extract : Write an ETL Function to Read the Three Data Files.
* **ETL_function_test.ipynb**
    - An ETL function is written to read in the three data files.
    - The function converts the Wikipedia JSON file to a Pandas DataFrame
  ![data-08-first-five-rows-of-wiki-movies-df-DataFrame_1](https://user-images.githubusercontent.com/75961057/146688381-a08c449b-ecd5-4e62-bc7e-be3bb5274707.png)

    - The function converts the Kaggle metadata file to a Pandas DataFrame
  ![data-08-first-five-rows-of-kaggle-metadata-DataFrame](https://user-images.githubusercontent.com/75961057/146688392-0304d30b-d9fa-4621-8bee-f9f57c46b20b.png)

    - The function converts the MovieLens ratings data file to a Pandas DataFrame
   ![data-08-ratings-DataFrame](https://user-images.githubusercontent.com/75961057/146688404-6ae4dcd1-0f1c-4169-97ed-2e12781b2269.png)

## Transform 
 * **ETL_clean_wiki_movies.ipynb: The transformation of the Wikipedia data in the ETL function** 
      - A list comprehension is used to keep columns with non-null values. 
      - The non-null box office data is converted to string values using the lambda and join functions.
      - A regular expression is used to match the six elements of "form_one" of the box office data. 
      - A regular expression is used to match the three elements of "form_two" of the box office data. 
      - The following columns are cleaned in the Wikipedia DataFrame: (8 pt)
           * The box office column
           * The budget column
           * The release date column
           * The running time column

![data-08-first-five-rows-of-wiki-movies-df-DataFrame_2](https://user-images.githubusercontent.com/75961057/146688569-bed02780-f87a-4ae4-b233-14804aadbc02.png)

![data-08-columns-of-the-wiki-movies-df-DataFrame](https://user-images.githubusercontent.com/75961057/146688586-3694e6a8-db76-44b0-a432-1b2b3b0b3f9a.png)

* **ETL_clean_kaggle_data.ipynb: Transform the Kaggle Data**

1. The extraction and transformation of the Kaggle metadata (movies_metadata.csv) using the ETL function does the following:
  - The Kaggle metadata is cleaned.
  - The Wikipedia and Kaggle DataFrames are merged
  - The following is performed on the merged Wikipedia and Kaggle DataFrames to create the movies_df
      * Unnecessary columns are dropped.
      * A function is used to fill in the missing Kaggle data.
      * The movies_df DataFrame is filtered to keep specific columns.
      * The movies_df DataFrame columns are renamed

![data-08-first-five-rows-of-the-movies-df-DataFrame](https://user-images.githubusercontent.com/75961057/146688905-87d1125b-d5aa-4b96-ad30-4f1dc195c44a.png)

2. The extraction and transformation of the MovieLens ratings data (ratings.csv) using the ETL function does the following:
  - The ratings counts are cleaned.
  - The movies_df DataFrame is merged with the cleaned ratings DataFrame to create the movies_with_ratings_df DataFrame. 
  - The empty values in the movies_with_ratings_df DataFrame are filled with “0”. 
  
  ![data-08-first-five-rows-of-movies-with-ratings-df-DataFrame](https://user-images.githubusercontent.com/75961057/146688846-41158d9d-4584-4092-bf72-a4d109d01ebc.png)
  
## Load
**ETL_create_database.ipynb: Create the Movie Database**
* The data from the movies_df DataFrame replaces the current data in the movies table in the SQL database, as determined by the movies_query.png. 
* The data from the MovieLens rating CSV file is added to the ratings table in the SQL database, as determined by the ratings_query.png. 
* The elapsed time to add the data to the database is displayed in the ETL_create_database.ipynb file.

<img width="561" alt="movies_query" src="https://user-images.githubusercontent.com/75961057/146689154-3f74f548-3899-4210-bb5e-2504234d8cb7.png">

<img width="406" alt="ratings_query" src="https://user-images.githubusercontent.com/75961057/146689162-991e3023-49da-44ac-9711-aae99eaf9950.png">

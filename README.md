# Equity and Film 

## Current Question
How is the percentage of female crew members changing over time? 

---
### Relevance
Diversity, equity, inclusion matters (see [slide 6](https://1drv.ms/p/s!AgY9SN2oit84kFzdyb4G3_nUja-r?e=r8Hg0Q)  )

---
### Resources:
* Source Code: [Keywords](keyword_ETL.ipynb), [Movies_Metadata](movies_metadata_ETL.ipynb), [Crew Credits](crew_ETL.ipynb)
* Source Data: [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?select=keywords.csv)
originally pulled from [TMDB](https://www.themoviesdb.org)
* Technology: [Python](https://www.python.org/), [Jupyter](https://jupyter.org/), [Amazon S3](https://aws.amazon.com/s3/?did=ap_card&trk=ap_card), [PostgreSQL](https://www.postgresql.org/), [pgAgmin](https://www.pgadmin.org/), [Pandas](https://pandas.pydata.org/), [scikit-learn](https://scikit-learn.org/stable/), [matplotlib](https://matplotlib.org/), [tableau](https://www.tableau.com/), [Excel](https://www.microsoft.com/en-us/?ql=3)
---
### Concept:
The source data includes many files, this project specifically examines the movie credits, keywords, and metadata. All three datasets contain a movie id which will be the unique identifier.

The **credits file** contains information for the crew. The crew data will be loaded into three tables based on department; Directing, Producing, and Writing. Looking at the gender will help to answer questions around equity, inclusion and diversity in the film industry. Additionally, this will provide context for relationships between crew members and keywords. 

The **keywords file** will provide context for themes and genres. Utilizing clusters to identify groups with potential for relationships such as female writers and common themes in film. 

The **metadata file** has several columns, this project focuses on; movie id, genre, release date, production company and production country. It will be interesting to compare trends between countries or production companies. 

---
### Analysis: 
Looking at 34,118 unique movies from 1950 through 2017, the data shows female crew members are increasing in the film industry. 

---
### Process:
The original dataset required significant cleaning before building a database. The metadata file was cleaned from 45,466 rows down to 41,328. First all films not yet released and adult films were dropped, then the columns of unnecessary data were dropped (from 23 columns down to 9). The cleaning continued by removing duplicate movie ids, and reordering the remaining columns. Then the release_date column was transformed to datetime and all movie id rows with a null value or before 01-01-1950 were dropped. The final row count was 41,328 unique movie ids. 

The metadata file also contained genres, companies and countries. Those columns were each a list of dictionaries that required exploding. All rows with null values were removed. The genres dataframe resulted in 83,259 rows, the countries dataframe resulted in 45,615 rows and the companies dataframe resulted in 66,355 rows. 

The keywords dataset started with 46,419 rows. First all rows with empty keywords were dropped, taking the dataset down to 31,624 rows. Then the keyword column, which was a list of dictionaries was exploded leaving one keyword dictionary per row. Then each row was split into columns; movie_id and keyword. Additionally it was important to track the occurance of each keyword, rows with keywords that occured less than 20 times were dropped. The resulting dataset is now 103,301 rows. 

Lastly the crew dataset started with 45,476 rows. First the cast column was dropped, then the crew column was exploded so that the list of dictionaries became one dictionary per row. From there, each dictionary row was split into many columns and unnecessary columns were dropped. The crew dataset has the gender column, the initial inspection showed there were 0s, 1s and 2s throughout. The 1 matched with females, 2 matched with males but 0 had inconsistencies that needed to be corrected. 

There were 272,319 crew members marked as 0, 31,123 members marked as 1 and 160,872 members marked as 2. By creating new dataframes with crew members marked as 0s, 1s, and 2s the dataframes were then merged on the intersection of 1 or 2. This yielded an additional 619 matches for gender 1 and 17,484 matches for gender 2. This project examines the proportion of female crew members so females remained gender '1' and males were transformed to '0'. After removing all remaining rows with unknown gender the clean crew dataset has 194,731 rows. The crew was then divided by department; Directing: 25,770 rows, Writing: 21,686 rows and Producing: 15,767.  

### Database Structure:

The database is hosted on Amazon S3, the cloud allows for quick and easy access for the team. The data has been divided into 8 tables. The structure allows for joins on the movie_id. 

The most important table for this project is the all_departments_percent table. This table has three columns; movie_id, release_date, percent_female. With the release date and percent of female crew members, one is able to track trends in the movie industry over the past 70 years. 

<img src="https://github.com/caseygomez/Capstone/blob/main/Images/all_depart_percent.png" height="250">

The keywords table has the most rows and one movie_id can have several keywords. It is important to capture the number of occurences to provide context for the other data sets. 

<img src="https://github.com/caseygomez/Capstone/blob/main/Images/keywords.png" height="250">

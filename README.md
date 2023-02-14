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
* Technology: [Pandas](https://pandas.pydata.org/), [Amazon S3](https://aws.amazon.com/s3/?did=ap_card&trk=ap_card), [PostgreSQL](https://www.postgresql.org/), [pgAgmin](https://www.pgadmin.org/), Excel, Python, scikit-learn, matplotlib, tableau
---
### Concept:
The source data includes many files, this project specifically examines the movie credits, keywords, and metadata. All three datasets contain a movie id which will be the unique identifier.

The **credits file** contains information for the cast as well as the crew. For this project the cast data will not be necessary. The crew data will be loaded into three tables based on department; Directing, Producing, and Writing. Looking at the gender will help to answer questions around equity, inclusion and diversity in the film industry. Additionally, this will provide context for relationships between cast members and keywords. 

The **keywords file** will provide context for themes and genres. Utilizing clusters to identify groups with potential for relationships such as female writers and common themes in film. 

The **metadata file** has several columns, this project focuses on; genre, title, production company, production country, original langauge, release date, vote average and vote count. It will be interesting to compare trends between countries or production companies. 

---
### Database Structure:

The database is hosted on Amazon S3, the cloud allows for quick and easy access for the team. The data has been divided into 8 tables. The structure allows for joins on the movie_id. 

The most important table for this project is the all_departments_percent table. This table has three columns; movie_id, release_date, percent_female. With the release date and percent of female crew members, one is able to track trends in the movie industry over the past 70 years. 

<img src="https://github.com/caseygomez/Capstone/blob/main/Images/all_depart_percent.png" height="400">

The keywords table has the most rows and one movie_id can have several keywords. It is important to capture the number of occurences to provide context to the other data sets. 

<img src="https://github.com/caseygomez/Capstone/blob/main/Images/keywords_table.png" height="400">

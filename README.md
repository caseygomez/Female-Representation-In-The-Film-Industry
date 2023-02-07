# Equity and Film 

## Current Questions
* Clustering of movie keywords by gender of movie crew
* Is cinema more inclusive in recent years? 
* Are there patterns around diversity and inclusion? 
* Has the percentage of female cast members increased over time? 

---
### Relevance
* Diversity, equity, inclusion matters (see [slide 6](https://1drv.ms/p/s!AgY9SN2oit84kFzdyb4G3_nUja-r?e=r8Hg0Q)  )

---
### Resources:
* Source Code: [Keywords](keyword_ETL.ipynb), [Movies_Metadata](movies_metadata.ipynb), [Credits](credits.ipynb)
* Source Data: [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?select=keywords.csv)
orginally pulled form [TMDB](https://www.themoviesdb.org)
* Technology: [Pandas](https://pandas.pydata.org/), [Amazon S3](https://aws.amazon.com/s3/?did=ap_card&trk=ap_card), [PostgreSQL](https://www.postgresql.org/), [pgAgmin](https://www.pgadmin.org/) 

---
### Concept:
The source data includes many files, this project specifically examines the movie credits, keywords, and metadata. All three datasets contain a movie id which will be the unique identifier.

The **credits file** contains information for the cast as well as the crew. For this project the cast data will not be necessary. The crew data will be loaded into three tables based on department; Directing, Producing, and Writing. Looking at the gender will help to answer questions around equity, inclusion and diversity in the film industry. Additionally, this will provide context for relationships between cast members and movie themes. 

The **keywords file** will provide context for themes and genres. Utilizing clusters to identify groups with potential for relationships such as female writers and common themes in film. 

The **metadata file** has several columns, this project focuses on; genre, title, production company, production country, original langauge, release date, vote average and vote count. It will be interesting to compare trends between countries or production companies. 

---
### Database Structure:

The database is hosted on Amazon S3, the cloud allows for quick and easy access for the team. The data has been divided into 8 tables. The structure allows for joins on the movie_id. 

The companies table consists of two columns, movie_id and companies. One movie_id can have many companies involved in the production. It will be interesting to see if there are trends around production companies and number of female crew members. 

<img src="https://github.com/caseygomez/Capstone/raw/images/Images/companies_table.png" height="400">

The countries table consists of three columns, movie_id, country code and country name. Is one country producing more films about diversity, equity or inclusion? 

<img src="https://github.com/caseygomez/Capstone/raw/images/Images/countries_table.png" height="400">

The genres table with provide context for determining the theme of the movie. One movie_id can be associated with more than one genre. 

<img src="https://github.com/caseygomez/Capstone/raw/images/Images/genre_table.png" height="400">

The keywords table has the most rows and one movie_id can have several keywords. It is important to capture the number of occurences to provide context to the other data sets. 

<img src="https://github.com/caseygomez/Capstone/raw/images/Images/keywords_table.png" height="400">

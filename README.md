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
* Source Code: [Keywords](keyword_ETL.ipynb), [Movies_Metadata](movies_metadata.ipynb)
* Source Data: [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?select=keywords.csv)
orginally pulled form [TMDB](https://www.themoviesdb.org)
* Technology: [Pandas](https://pandas.pydata.org/), [Amazon S3](https://aws.amazon.com/s3/?did=ap_card&trk=ap_card), [PostgreSQL](https://www.postgresql.org/), [pgAgmin](https://www.pgadmin.org/) 

---
### Concept:
The source data includes many files, this project specifically examines the movie credits, keywords, and metadata. All three datasets contain a movie id which will be the unique identifier.

The **credits file** contains information for the cast as well as the crew. For this project the cast data will not be necessary. The crew data will be loaded into three tables based on department; Directing, Producing, and Writing. These tables will include the movie id, gender of the crew member and their job. Looking at the gender will help to answer questions around equity, inclusion and diversity in the film industry. Additionally, this will provide context for relationships between cast members and movie themes. 

The **keywords file** will be cleaned and loaded with the movie id and keywords associated with the film. This will provide context for themes and genres. Utilizing clusters to identify groups with potential for relationships such as female writers and common themes in film. 

The **metadata file** has several columns, this project focuses on; genre, title, production company, production country, original langauge, release date, vote average and vote count. It will be interesting to compare trends between countries or production companies. 


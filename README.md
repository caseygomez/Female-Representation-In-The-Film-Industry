# Analysis Of Female Representation In The Film Industry

Movies are a vehicle for audiences to experience different emotions and see new perspectives. The worldview, beliefs, and economic interests of those making the movies will be reflected in the stories being told. As such, it is important that women are included in the filmmaking process so that their voices and stories can be heard. 

---
### Current Question
Has there been an increase in the percentage of females employed in the film industry since 1950?

---
### Resources

* Source Code: [Movies_Metadata](movies_metadata_ETL.ipynb), [Crew Credits](crew_ETL.ipynb), [Keywords](keyword_ETL.ipynb).

* Source Data: [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?select=keywords.csv)
originally scraped from [TMDB](https://www.themoviesdb.org)

* Technology: [Python](https://www.python.org/), [Jupyter](https://jupyter.org/), [Amazon RDS](https://aws.amazon.com/rds/?did=ap_card&trk=ap_card), [PostgreSQL](https://www.postgresql.org/), [pgAgmin](https://www.pgadmin.org/), [Pandas](https://pandas.pydata.org/), [scikit-learn](https://scikit-learn.org/stable/), [matplotlib](https://matplotlib.org/), [tableau](https://www.tableau.com/), [Excel](https://www.microsoft.com/en-us/?ql=3)

---
### Summary
A linear regression model was used to describe the percentage of female crew member participation through time by film department (directing, writing, and production), as well as the combination of those three target departments. 

The model showed that there is a positive correlation between the variables of percentage of females employed and time. Thus, we can conclude that the percentage of females has increased since the 1950s and will continue to increase. Using the equations produced in this analysis, we can predict the year that females will make up 50% of film industry crew members in the target departments.

<img width=100% alt="equity_predictions" src="https://user-images.githubusercontent.com/111674383/221101049-d901efe1-1514-489b-9798-3708a21287f9.png">

---
## Process

### Data Preparation
The three files chosen from the original dataset required significant cleaning before they could be used to build the database. 

#### Metadata
The metadata file, before cleaning, contained **45,466** rows. First, all adult films and films that had not yet been released were dropped. Then, the columns of unnecessary data were dropped, reducing the total columns from 23 down to 9. The cleaning continued by removing duplicate movie ids and reordering the remaining columns. Then the release date column was transformed from a string to a datetime format, and all movie ids before 01-01-1950 were dropped. In total, the metadata file was reduced down to **34,118** unique movie ids. 

The metadata file also contained information on movie genres, production companies, and the countries the films were produced in. Those columns were each a list of dictionaries that required exploding. All rows with null values were removed, and separate dataframes were created for each of those topics. The genres dataframe resulted in **83,259** rows, the countries dataframe resulted in **45,615** rows, and the companies dataframe resulted in **66,355** rows. 

#### Credits
The credits dataset started with **45,476** rows and contained a column of data that contained the details of each cast member and another column with the details of each crew member for each movie id. First, the cast column was dropped and the crew column was exploded, so that the list of dictionaries became one dictionary per row. From there, the key/value pairs in each dictionary were turned into individual columns, and unnecessary columns were dropped. 

The crew dataset contained the gender information, and initial inspection showed that **272,319** crew members were assigned a gender of 0, **31,123** members were assigned a 1, and **160,872** members were assigned a 2. It was determined that 1 indicated female and 2 indicated male, but the dataset did not explain what the assignment of 0 indicated. Further investigation revealed that a subset of people marked as 0 were also listed as either 1 or 2 under different movie ids. By creating separate dataframes for crew members marked as 0, 1, and 2, the intersection of groups 0 and 1 were evaluated to find crew members who could be reassigned gender 1. This yielded an additional **619** matches for gender 1. The same process was repeated for groups 0 and 2, yielding an additional **17,484** matches for gender 2. All gender 0 crew members who could not be assigned to groups 1 or 2 were dropped. After this gender assignment process was completed, the clean crew dataset had **194,731** rows. The crew was then separated for further analysis by the target departments, so that there were **25,569** rows in the directing department, **21,558** rows in the writing department, and **15,675** rows in the production department. 

In order to perform the final calculations necessary for this project, one final gender reassignment process had to take place. The gender assignment for females remained 1, but all males were transformed to 0. This allowed the dataset to be grouped by movie id, and then the percentage of female crew members for each movie was calculated by dividing the sum of all crew members by the count of all crew members. 

#### Keywords
The keywords dataset started with **46,419** rows. First, all rows with empty keywords were dropped, taking the dataset down to **31,624** rows. Then, the keyword column, which was a list of dictionaries, was exploded so that there was one dictionary per row. Then each key/value pair was split into separate columns for the movie id and keyword. The resulting dataset contained **158,516** rows and **18,638** unique keywords. This data was further condensed by grouping similar terms under broader keyword phrases (e.g., "dying woman", "man on deathbed", and "presumed dead" were all reclassified as "death and dying"). Rows with overly specific keywords (e.g., "character's point of view camera shot") or keywords indicating film genre were dropped. The final keyword file contained **145,494** rows and **14,243** unique keywords.

### Data Manipulation
The cleaned files were loaded into tables in Postgres, and the database was hosted on Amazon RDS to allow quick and easy access for all team members. 

The tables were joined on movie id, and the final file utilized for machine learning contained the release date from the metadata file and the percentage of female crew members calculated with the credits file. 

---
## Machine Learning

### Model Selection
A linear regression model was selected to assess the percentage of female crew members in the film industry over time. Linear regression evaluates the character and strength of the association between an independent variable and a corresponding dependent variable. The result of the mathematical association between female participation over time shows if the number of females working in movies has increased, decreased, or stayed relatively constant throughout the years. This provided a clear insight as to the general behavior of the dataset within the parameters of the time and resources allocated for the project.

Additionally, the linear regression equation can be used to predict new values based on the existing data. The percentage of female participation within the evaluated departments is a continuous numerical value that matches the requirements for the linear regression model.

### Linear Regression Equation, Accuracy, And Results
The linear regression model was applied to the following datasets.

* Female participation in the combined target departments (Directing, Production, and Writing)
* Female participation by individual department (Directing, Production, and Writing)

#### Percentage Of Female Crew Across Combined Target Departments (Directing, Production, and Writing) Over Time

##### *Linear regression model*
![all_depart_model](https://github.com/caseygomez/Capstone/blob/main/Images/all_depart_model.png)

##### *Model calculated error*
![all_depart_error](https://github.com/caseygomez/Capstone/blob/main/Images/all_depart_error.png)

##### *Residuals graph*
![all_depart_residuals](https://github.com/caseygomez/Capstone/blob/main/Images/all_depart_residuals.png)

#### Percentage Of Female Crew In The Directing Department Over Time

##### *Linear regression model*
![directing_model](https://github.com/caseygomez/Capstone/blob/main/Images/directing_model.png)

##### *Model calculated error*
![directing_error](https://github.com/caseygomez/Capstone/blob/main/Images/directing_error.png)

##### *Residuals graph*
![directing_residuals](https://github.com/caseygomez/Capstone/blob/main/Images/directing_residuals.png)

#### Percentage Of Female Crew In The Production Department Over Time

##### *Linear regression model*
![production_model](https://github.com/caseygomez/Capstone/blob/main/Images/production_model.png)

##### *Model calculated error*
![production_error](https://github.com/caseygomez/Capstone/blob/main/Images/production_error.png)

##### *Residuals graph*
![production_residuals](https://github.com/caseygomez/Capstone/blob/main/Images/production_residuals.png)

#### Percentage Of Female Crew In The Writing Department Over Time

##### *Linear regression model*
![writing_model](https://github.com/caseygomez/Capstone/blob/main/Images/writing_model.png)

##### *Model calculated error*
![writing_error](https://github.com/caseygomez/Capstone/blob/main/Images/writing_error.png)

##### *Residuals graph*
![writing_residuals](https://github.com/caseygomez/Capstone/blob/main/Images/writing_residuals.png)

---
### Tableau Visualizations
Using Tableau connections, all of the .csv files produced in Postgres were linked via the movie id column in the metadata table. The key metrics that were evaluated in the dashboard were movie release date, average percentage female crew members, genre, keyword, production countries, and production companies. 

![all_departmentes_line](https://github.com/caseygomez/Capstone/blob/main/Images/tableau_all.png)

*Genres By Percentage Of Female Crew*
![genres](https://github.com/caseygomez/Capstone/blob/main/Images/tableau_genres.png)

*Keyword By Romance Genre*
![Keyword by Romance](https://github.com/caseygomez/Capstone/blob/main/Images/tableau_keywords_romance.png)

---
### Future Analysis 
* Nonlinear regression could be explored as a way to better describe the relationship between female crew members and time. 
* The currently cleaned and prepared data could be used to explore deeper connections between female crew members and keywords, genres, and production countries and companies. 
* The formerly deleted cast column could be cleaned and prepared using the same steps from the current analysis.
  * The cast data could be used to explore the same relationships proposed for the crew data.
  * Female cast and crew data could be investigated for possible correlations. 
* The cleaned files also contain information on average voting scores for each movie, which could be analyzed for relationships between scores and female cast or crew members.
* The Movies Database API could be scraped for data on movies released between 2017 and present day. This additional data could be used to validate the predictions already made in this analysis, as well as provide additional data points to refine the analysis.

---
### Additional Materials
* [Google Slidedeck - Project Presentation](https://docs.google.com/presentation/d/1cCyO-_hIM7on5ASuPeHfUKymAdPlYvNrsFdtkkTZsNc/edit?usp=sharing)
* [Tableau Story](https://public.tableau.com/shared/NZMPFXXD3?:display_count=n&:origin=viz_share_link)


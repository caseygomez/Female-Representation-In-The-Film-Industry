# Analysis Of Female Representation In The Film Industry

Movies are a vehicle for audiences to experience different emotions and see new perspectives. The worldview, beliefs and economic interests of those making the movies will be reflected in the stories being told. This is why it is important that women are included in the filmmaking process.

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
A linear regression model was used to describe the percentage of female crew member participation through time. 

The model showed that there is a positive correlation between the variables of percentage of females employed and time, thus we can conclude that the percentage of females has increased since the 1950s and will continue to increase.

---
### Process
The original dataset required significant cleaning before building a database. The metadata file was cleaned from **45,466** rows down to **34,118**. First, all films not yet released and adult films were dropped. Then the columns of unnecessary data were dropped from 23 columns down to 9. The cleaning continued by removing duplicate movie ids, and reordering the remaining columns. Then the release_date column was transformed to datetime and all movie id rows with a null value or before 01-01-1950 were dropped. The final row count was **34,118** unique movie ids. 

The metadata file also contained genres, companies, and countries. Those columns were each a list of dictionaries that required exploding. All rows with null values were removed, and separate dataframes were created for each of those topics. The genres dataframe resulted in **83,259** rows, the countries dataframe resulted in **45,615** rows, and the companies dataframe resulted in **66,355** rows. 

The keywords dataset started with **46,419** rows. First, all rows with empty keywords were dropped, taking the dataset down to **31,624** rows. Then, the keyword column, which was a list of dictionaries, was exploded so that there was one dictionary per row. Then each key/value pair was split into columns the movie_id and keyword. The resulting dataset was **103,301** rows. 

Lastly, the crew dataset started with **45,476** rows. First, the cast column was dropped and the crew column exploded so that the list of dictionaries became one dictionary per row. From there, the key/value pairs in each dictionary were turned into individual columns, and unnecessary columns were dropped. The crew dataset contained the gender information, and initial inspection showed that crew members were assigned a 0, 1, or 2 to denote gender. The 1 matched with females, 2 matched with males, but 0 had inconsistencies that needed to be corrected. 

There were **272,319** crew members marked as 0, **31,123** members marked as 1 and **160,872** members marked as 2. By creating new data frames with crew members marked as 0s, 1s, and 2s, the data frames were then merged on the intersection of 1 or 2. This yielded an additional **619** matches for gender 1 and **17,484** matches for gender 2. All gender 0 crew members who could not be assigned to groups 1 or 2 were dropped.

In order to perform the calculations necessary for this project, the gender assignment for females remained 1, but all males were transformed to 0. After the gender assigment process was completed, the clean crew dataset had **194,731** rows. The crew was then divided by department; Directing: **25,569** rows, Writing: **21,558** rows and Production: **15,675**.  

---
### Database Structure

The database is hosted on Amazon RDS, the cloud allows for quick and easy access for the team. The structure allows for joins on the movie_id. 

---
## Machine Learning

### Model Selection
For the assessment of female participation in the film industry through the years a linear regression model was selected. Linear regression evaluates the character and strength of the association between an independent variable and a corresponding dependent variable. The result of the mathematical association between female participation over time would show if the number of females working in movies has increased, decreased or stayed relatively constant through the years. This would provide a clear insight on the general behavior of the dataset with the resources and time allocated for this project.

Additionally, the linear regression equation can be used to predict new values based on the existing data. The percentage of female participation within the evaluated departments is a continuous numerical value that matches the requirements for the linear regression model.
As can be showed below, a linear regression model was applied to the following datasets over time:
* Female participation in all departments (Directing, Production, and Writing)
* Female participation by individual department (Directing, Production, and Writing)

### Linear Regression Equation And Accuracy

Below the obtained supervised models are described: 

* Female participation in all departments (Directing, Production, and Writing)

*Linear regression model*

![all_depart_model](https://github.com/caseygomez/Capstone/blob/accuracy/Images/all_depart_model.png)

*Model calculated error*

![all_depart_error](https://github.com/caseygomez/Capstone/blob/accuracy/Images/all_depart_error.png)

*Residuals graph*

![all_depart_residuals](https://github.com/caseygomez/Capstone/blob/accuracy/Images/all_depart_residuals.png)

* Female participation in the Directing department

*Linear regression model*

![directing_model](https://github.com/caseygomez/Capstone/blob/accuracy/Images/directing_model.png)

*Model calculated error*

![directing_error](https://github.com/caseygomez/Capstone/blob/accuracy/Images/directing_error.png)

*Residuals graph*

![directing_residuals](https://github.com/caseygomez/Capstone/blob/accuracy/Images/directing_residuals.png)

* Female participation in the Production department

*Linear regression model*
![production_model](https://github.com/caseygomez/Capstone/blob/accuracy/Images/production_model.png)

*Model calculated error*

![production_error](https://github.com/caseygomez/Capstone/blob/accuracy/Images/production_error.png)

*Residuals graph*

![production_residuals](https://github.com/caseygomez/Capstone/blob/accuracy/Images/production_residuals.png)


* Female participation in the Writing department

*Linear regression model*
![writing_model](https://github.com/caseygomez/Capstone/blob/accuracy/Images/writing_model.png)

*Model calculated error*

![writing_error](https://github.com/caseygomez/Capstone/blob/accuracy/Images/writing_error.png)

*Residuals graph*

![writing_residuals](https://github.com/caseygomez/Capstone/blob/accuracy/Images/writing_residuals.png)

---
### Tableau 
Using Tableau connections, all csv files produced from the database were linked via the movie id column from the metadata table. There were key values that were consistently used throughout the dashboard; release date, average percentage female, keyword, countries, companies. 

*All Departments graph*

---
### Future Analysis 
In the future, the cleaned data could be used to explore deeper connections between keywords, countries, genres, and companies. Additionally, exploring nonlinear regression could better describe the relationship between female crew members and time. 

---
### Additional Materials
[Google Slidedeck - Project Presentation](https://docs.google.com/presentation/d/1cCyO-_hIM7on5ASuPeHfUKymAdPlYvNrsFdtkkTZsNc/edit?usp=sharing)


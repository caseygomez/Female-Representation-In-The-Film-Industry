# Flowchart

**CrewETL**
	- takes credits.csv, explodes, cleans, assigns gender properly, converts male to 0 instead of 2, splits clean data into files
        - creates clean_dept_directing.csv, clean_dept_writing.csv, clean_dept_production.csv, and clean_dept_all.csv
            - contains movie_id, department, gender, job, and name

    - calculates sum, total, and percent female for each movie_id and department
        - creates gender_directing.csv, gender_writing.csv, gender_production.csv, and gender_all.csv
            - contains movie_id, sum_gender, total_gender, and percent_female








# SQL Queries

**Create tables from clean_dept csv files**
(Should have included not null for all rows!)

create table clean_dept_all (
	movie_id int not null,
	department varchar (50),
    gender float,
    job varchar (50),
    name varchar (250)
)

**Create tables from gender csv files**

create table gender_all (
	movie_id int not null,
	sum_gender float not null,
    total_gender float not null,
    percent_female float not null
    primary key (movie_id)
)

**Join metadata (release_date, movie_id) and gender(percent_female) into percent tables**
    - contains movie_id, release_date, and percent_female

select md.movie_id,
	md.release_date,
	ga.percent_female
into percent_all
from gender_all as ga
inner join metadata as md
	on(md.movie_id=ga.movie_id)










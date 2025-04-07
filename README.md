# Netflix Movies and TV Shows Data Analysis using SQL

![Netflix Logo](https://github.com/Meet8844/netflix_sql_project/blob/main/logo.png)

## Overview
This project presents an in-depth analysis of Netflix's movie and TV show dataset using SQL. The primary objective is to derive meaningful insights and address key business questions through data exploration. This README outlines the project's goals, the business challenges examined, the analytical approach taken, key discoveries, and final conclusions.

## Objective
- Examine the distribution between movies and TV shows to understand content composition.
- Determine the most frequently assigned ratings across both content types.
- Analyze content trends by release year, country of origin, and duration.
- Segment and evaluate content based on defined criteria and relevant keywords for deeper insights.

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:** [Movies Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)

## Schema
```sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
 	show_id		VARCHAR(6),
 	type		VARCHAR(10),
	title		VARCHAR(150),
	director	VARCHAR(208),
	casts		VARCHAR(1000),
	country		VARCHAR(150),
	date_added	VARCHAR(50),
	release_year	INT,
	rating		VARCHAR(10),
	duration	VARCHAR(15),
	listed_in	VARCHAR(100),
	description	VARCHAR(250)
);
```

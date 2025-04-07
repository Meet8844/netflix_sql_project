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

## Business Problems and Solutions

### 1. Count the number of Movies VS TV Shows.
```sql
SELECT 
	type, 
	COUNT(*) as total_content
FROM netflix
GROUP BY type;
```

**Objective:** Determine the distribution of content types on Netflix.

### 2. Find the most common rating for movies and TV shows.
```sql
SELECT
	type, 
	rating
FROM
(
		SELECT 
			type,
			rating,
			COUNT(*),
			RANK() OVER(PARTITION BY type ORDER BY COUNT(*) DESC) as ranking
		FROM netflix
		GROUP BY 1, 2
) as table1
WHERE 
	ranking = 1;
```

**Objective:** Identify the most frequently occurring rating for each type of content.

### 3. List all movies released in a specific year (e.g., 2020).
```sql
SELECT 
    *
FROM
    netflix
WHERE
    type = 'Movie' AND release_year = 2020;
```

**Objective:** Retrieve all movies released in a specific year.

### 4. Find the top 5 countries with the most content on netflix.
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(country, ',')) AS new_country,
	COUNT(show_id) as total_content
FROM netflix
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

**Objective:** Identify the top 5 countries with the highest number of content items.

### 5. Identify the longest movie or TV show duration.
```sql
SELECT 
    *
FROM
    netflix
WHERE
    type = 'Movie'
        AND duration = (SELECT 
            MAX(duration)
        FROM
            netflix);
```

**Objective:** Find the movie with the longest duration.

### 6. Find content added in last five years.
```sql
SELECT *
FROM netflix
WHERE
	TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5years';
```

**Objective:** Retrieve content added to Netflix in the last 5 years.


### 7. Find all the movies/TV shows by director 'Rajiv Chilaka'!
```sql
SELECT 
    *
FROM
    netflix
WHERE
    director LIKE '%Rajiv Chilaka%';
```

**Objective:** List all content directed by 'Rajiv Chilaka'.

### 8. List all TV shows with more than 5 seasons.
```sql
SELECT 
	type,
	duration
FROM netflix
WHERE 
	type = 'TV Show'
	AND
	SPLIT_PART(duration, ' ', 1)::numeric > 5;
```

**Objective:** Identify TV shows with more than 5 seasons.

### 9.Count the number of content items in each genre.
```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,
    COUNT(show_id) AS total_content
FROM
    netflix
GROUP BY 1;
```

**Objective:** Count the number of content items in each genre.

### 10.Find each year and the average release number of content release by India on netflix. Return top 5 year with highest avg content release!
````sql
-- total content 333/972
SELECT 
	EXTRACT(YEAR FROM TO_DATE(date_added, 'Month DD, YYYY')) as year,
	COUNT(*) as yearly_content,
	ROUND(
	COUNT(*)::numeric/(SELECT COUNT(*) FROM netflix WHERE country = 'India'):: numeric * 100 
	,2) as avg_content_per_year
FROM netflix
WHERE country = 'India'
GROUP BY 1;
````

**Objective:** Calculate and rank years by the average number of content releases by India.

### 11. List all movies that are documentaries.
```sql
SELECT * FROM netflix
WHERE listed_in ILIKE '%documentaries%';
```

**Objective:** Retrieve all movies classified as documentaries.

### 12. Find all content without a director.
```sql
SELECT * FROM netflix
WHERE director IS NULL;
```

**Objective:** List content that does not have a director.


### 13. Find how many movies actor 'Salman Khan' appeared in last 10 years!
```sql
SELECT * FROM netflix
WHERE 
	casts ILIKE '%Salman Khan%'
	AND
	release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

**Objective:** Count the number of movies featuring 'Salman Khan' in the last 10 years.


### 14. Find the top 10 actors who have appeared in the heighest number of movies produced in India.
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(casts, ',')) as actors,
	COUNT(*) as total_content
FROM netflix
WHERE country ILIKE '%india'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```

**Objective:** Identify the top 10 actors with the most appearances in Indian-produced movies.


### 15.Categorize the content based on the presence of the keywords 'killi and 'violence' in the descripti on field. Label content containing these keywords as 'Bad' and all other content as 'Good'. Count how many items fall into each category.
```sql
WITH new_table
AS
(
SELECT
*,
	CASE
	WHEN 
		description ILIKE '%kill%' OR
		description ILIKE '%violence%' THEN 'Bad_Content'
		ELSE 'Good_Content'
	END as category
FROM netflix
)
SELECT
	 category,
	 COUNT(*) as total_content
FROM new_table
GROUP BY 1;
```

**Objective:** Categorize content as 'Bad' if it contains 'kill' or 'violence' and 'Good' otherwise. Count the number of items in each category.

## Findings and Conclusion

- **Content Overview:** The dataset encompasses a wide array of movies and TV shows, reflecting diversity in both ratings and genres.
- **Rating Distribution Analysis:** Identifying the most frequent ratings offers valuable insights into the intended audience demographics.
- **Regional Distribution Patterns:** Analysis of leading content-producing countries, with a focus on India, reveals trends in regional content output.
- **Thematic Classification:** Leveraging keyword-based categorization enables a deeper understanding of the thematic composition of Netflix's content library.

This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.

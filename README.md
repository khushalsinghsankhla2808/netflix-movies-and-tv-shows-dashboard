# 📊 Netflix Data Analysis using PostgreSQL

## 📌 Overview

This project focuses on analyzing a Netflix dataset using PostgreSQL to extract meaningful insights and solve real-world business problems. The dataset includes details about movies and TV shows such as title, genre, country, cast, release year, ratings, and descriptions.

---

## 🚀 Key Features

* Performed analysis on Movies vs TV Shows distribution
* Identified most common ratings by content type
* Found top countries producing Netflix content
* Analyzed yearly trends in content release
* Extracted insights on actors, directors, and genres
* Categorized content based on keywords like *Kill* and *Violence*

---

## 🛠️ SQL Concepts Used

* Aggregation Functions: `COUNT()`, `MAX()`
* Window Functions: `RANK()`
* String Functions: `STRING_TO_ARRAY()`, `UNNEST()`, `SPLIT_PART()`
* Subqueries & Common Table Expressions (CTEs)
* Conditional Logic: `CASE WHEN`
* Date Functions: `TO_DATE()`, `EXTRACT()`

---

# 📊 Netflix Data Analysis using PostgreSQL — SQL Queries

This section contains all SQL queries used in the project with proper formatting and structured solutions.

---

## 🧱 1. Create Netflix Table

```sql
DROP TABLE IF EXISTS netflix;

CREATE TABLE netflix (
    show_id      VARCHAR(20),
    type         VARCHAR(10),
    title        VARCHAR(500),
    director     VARCHAR(500),
    casts        VARCHAR(1000),
    country      VARCHAR(500),
    date_added   VARCHAR(50),
    release_year INT,
    rating       VARCHAR(10),
    duration     VARCHAR(15),
    listed_in    VARCHAR(500),
    description  VARCHAR(300)
);
```

---

## 📌 2. Count Total Records

```sql
SELECT 
    COUNT(*) AS total_records
FROM netflix;
```

---

## 🎬 3. Movies vs TV Shows Count

```sql
SELECT
    type,
    COUNT(*) AS total_count
FROM netflix
GROUP BY type;
```

---

## ⭐ 4. Most Common Rating by Type

```sql
SELECT
    type,
    rating,
    total,
    ranking
FROM (
    SELECT
        type,
        rating,
        COUNT(*) AS total,
        RANK() OVER (
            PARTITION BY type
            ORDER BY COUNT(*) DESC
        ) AS ranking
    FROM netflix
    GROUP BY type, rating
) t
WHERE ranking = 1;
```

---

## 🎥 5. Movies Released in Specific Year

```sql
SELECT *
FROM netflix
WHERE type = 'Movie'
AND release_year = 2020;
```

---

## 🌍 6. Top 5 Countries with Most Content

```sql
SELECT
    TRIM(UNNEST(STRING_TO_ARRAY(country, ','))) AS country_name,
    COUNT(show_id) AS total_count
FROM netflix
WHERE country IS NOT NULL
GROUP BY 1
ORDER BY total_count DESC
LIMIT 5;
```

---

## 🎞️ 7. Longest Movie

```sql
SELECT *
FROM netflix
WHERE type = 'Movie'
AND duration = (
    SELECT MAX(duration)
    FROM netflix
    WHERE type = 'Movie'
);
```

---

## 📅 8. Content Added in Last 5 Years

```sql
SELECT
    type,
    COUNT(*) AS total_content
FROM netflix
WHERE date_added IS NOT NULL
AND TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years'
GROUP BY type;
```

---

## 🎬 9. Content by Director (Example: Rajiv Chilaka)

```sql
SELECT *
FROM netflix
WHERE director ILIKE '%Rajiv Chilaka%';
```

---

## 📺 10. TV Shows with More Than 5 Seasons

```sql
SELECT *
FROM netflix
WHERE type = 'TV Show'
AND SPLIT_PART(duration, ' ', 1)::INT > 5;
```

---

## 🎭 11. Genre Count

```sql
SELECT
    TRIM(UNNEST(STRING_TO_ARRAY(listed_in, ','))) AS genre,
    COUNT(show_id) AS total_count
FROM netflix
WHERE listed_in IS NOT NULL
GROUP BY 1
ORDER BY total_count DESC;
```

---

## 🇮🇳 12. India Content Trend by Year

```sql
SELECT
    release_year,
    COUNT(*) AS total_content
FROM netflix
WHERE country ILIKE '%India%'
GROUP BY release_year
ORDER BY total_content DESC
LIMIT 5;
```

---

## 🎬 13. Documentaries (Movies Only)

```sql
SELECT
    title,
    type,
    genre
FROM (
    SELECT
        title,
        type,
        TRIM(UNNEST(STRING_TO_ARRAY(listed_in, ','))) AS genre
    FROM netflix
) t
WHERE type = 'Movie'
AND genre = 'Documentaries';
```

---

## ❌ 14. Content Without Director

```sql
SELECT *
FROM netflix
WHERE director IS NULL;
```

---

## 🎭 15. Actor Appearance (Salman Khan Example)

```sql
SELECT *
FROM netflix
WHERE casts ILIKE '%Salman Khan%'
AND release_year >= EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

---

## 🎬 16. Top Actors in Indian Movies

```sql
SELECT
    TRIM(UNNEST(STRING_TO_ARRAY(casts, ','))) AS actor,
    COUNT(*) AS total_movies
FROM netflix
WHERE type = 'Movie'
AND country ILIKE '%India%'
GROUP BY 1
ORDER BY total_movies DESC
LIMIT 10;
```

---

## ⚠️ 17. Content Classification (Good vs Bad)

```sql
WITH categorized_content AS (
    SELECT
        *,
        CASE
            WHEN description ILIKE '%kill%'
              OR description ILIKE '%violence%'
            THEN 'BAD_CONTENT'
            ELSE 'GOOD_CONTENT'
        END AS category
    FROM netflix
)
SELECT
    category,
    COUNT(*) AS total_count
FROM categorized_content
GROUP BY category;
```

---

# 🚀 End of SQL Queries

This project demonstrates advanced SQL techniques including:

* Aggregations
* Window Functions
* String Manipulation
* CTEs
* Data Cleaning & Transformation


## 📈 Outcomes

* Derived actionable insights from raw data
* Handled multi-valued columns (genres, cast, country)
* Improved data analysis and SQL problem-solving skills
* Built a strong foundation for real-world data analytics projects

---

## 🎯 Conclusion

This project demonstrates practical SQL skills in data cleaning, transformation, and analysis using PostgreSQL. It showcases the ability to work with real datasets and solve business-oriented problems, making it suitable for data analytics and backend roles.

---

## 🌐 Connect With Me

**Khushal Singh Sankhla**
- GitHub: [khushal280804](https://github.com/khushal280804)
- LinkedIn: [Khushal Singh Sankhla](https://www.linkedin.com/in/khushal-singh-sankhla/)
- Email: khushalsinghsankhla203@gmail.com

For queries, feedback, or collaboration, feel free to reach out.

⭐ Feel free to explore and contribute!
AUTHOR 
KHUSHAL SINGH SANKHLA

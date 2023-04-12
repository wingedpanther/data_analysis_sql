# Olympic Data Analysis
This is a repository containing queries to analyze the 120 years of Olympic history dataset available on Kaggle.

## Data Preparation
I have used Postgres ver 15. The data can be downloaded from [Kaggle](https://www.kaggle.com/heesoo37/120-years-of-olympic-history-athletes-and-results/download).

To create a table in PostgreSQL and copy the downloaded data, use the following code:

```
CREATE TABLE IF NOT EXISTS OLYMPICS_HISTORY
(
    id          INT,
    name        VARCHAR,
    sex         VARCHAR,
    age         VARCHAR,
    height      VARCHAR,
    weight      VARCHAR,
    team        VARCHAR,
    noc         VARCHAR,
    games       VARCHAR,
    year        INT,
    season      VARCHAR,
    city        VARCHAR,
    sport       VARCHAR,
    event       VARCHAR,
    medal       VARCHAR
);

```

Import(copy) the downloaded data into the *OLYMPICS_HISTORY* table

```
COPY olympics_history FROM 'path/to/csv/file.csv' DELIMITER ',' CSV HEADER;
```

## Data Cleaning    

### Checking the missing values aka null rows
```
SELECT COUNT(*) 
FROM OLYMPICS_HISTORY 
WHERE id IS NULL OR name IS NULL OR sex IS NULL OR age IS NULL 
    OR height IS NULL OR weight IS NULL OR team IS NULL 
    OR noc IS NULL OR games IS NULL OR year IS NULL 
    OR season IS NULL OR city IS NULL OR sport IS NULL 
    OR event IS NULL OR medal IS NULL;

```

### Checking duplicates and removing them

```
SELECT COUNT(*) duplicate_rows
FROM (
    SELECT id, name, sex, age, height, weight, team, noc, games, year, season, city, sport, event, medal, 
        ROW_NUMBER() OVER (PARTITION BY id, name, sex, age, height, weight, team, noc, games, year, season, city, sport, event, medal ORDER BY id) AS rn
    FROM OLYMPICS_HISTORY
) sub
WHERE rn > 1;

```
### Deleting the duplicate rows
```
DELETE FROM OLYMPICS_HISTORY 
WHERE id IN (
    SELECT id
    FROM (
        SELECT id, name, sex, age, height, weight, team, noc, games, year, season, city, sport, event, medal, 
            ROW_NUMBER() OVER (PARTITION BY id, name, sex, age, height, weight, team, noc, games, year, season, city, sport, event, medal ORDER BY id) AS rn
        FROM OLYMPICS_HISTORY
    ) sub
    WHERE rn > 1
);


```

## Data Exploration

### Finding the total number of athletes who participated in each Olympic game

```
SELECT games, COUNT(DISTINCT id) AS num_athletes
FROM OLYMPICS_HISTORY
GROUP BY games
ORDER BY games;

```

### Finding the total number of medals won by each country

```
SELECT noc, COUNT(medal) AS num_medals
FROM OLYMPICS_HISTORY
WHERE medal IS NOT NULL
GROUP BY noc
ORDER BY num_medals DESC limit 10;

```
## Basic Statistics

### Finding the average age of male and female athletes

```
SELECT sex, round(AVG(age::int)) AS avg_age
FROM OLYMPICS_HISTORY
where age <> 'NA'
GROUP BY sex;
```

### Finding the standard deviation of height and weigh
```
SELECT STDDEV(height::numeric) AS sd_height, STDDEV(weight::numeric) AS sd_weight
FROM OLYMPICS_HISTORY
where height <> 'NA' and weight <> 'NA';
```

## Data Vizualization

*Note: I have used metabase to vizualize the data*
### Age distribution
```
SELECT age, COUNT(*) AS num_athletes
FROM OLYMPICS_HISTORY
WHERE age IS NOT NULL
GROUP BY age
ORDER BY age;
```

```
SELECT height, weight
FROM OLYMPICS_HISTORY
WHERE height IS NOT NULL AND weight IS NOT NULL;
```

## Hypothesis Analysis
### To find if there is a significant difference in the average age of medalists and non-medalists

*The actual data stored Age as text, you can cast the Age in runtime or make chnages to the entire column. I have used the former*
```
SELECT medal, round(AVG(age::int)) AS avg_age, COUNT(*) AS num_athletes
FROM OLYMPICS_HISTORY
WHERE age IS NOT NULL and age <> 'NA'
GROUP BY medal;
```
### To find if there is a significant difference in the average height of male and female athletes

```
SELECT sex, AVG(height::numeric) AS avg_height, COUNT(*) AS num_athletes
FROM OLYMPICS_HISTORY
WHERE height IS NOT NULL and height <> 'NA'
GROUP BY sex;
```

### Time series
### Number of medals won by country over time

```
SELECT noc, year, COUNT(*) AS count_medals
FROM olympics_history
WHERE medal IS NOT NULL
GROUP BY noc, year;
```

## Predict the likelihood of winning a medal based on age, height, weight, and sport using logistic regression

```
SELECT age, height, weight, sport, 
       CASE WHEN medal IS NOT NULL THEN 1 ELSE 0 END AS medal_won
FROM olympics_history;

```

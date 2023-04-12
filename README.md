# Olympic Data Analysis
This is a repository containing queries to analyze the 120 years of Olympic history dataset available on Kaggle.

## Data Preparation
The data can be downloaded from [Kaggle](https://www.kaggle.com/heesoo37/120-years-of-olympic-history-athletes-and-results/download).

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
### Deleting the duplicate rows
```
SELECT COUNT(*) 
FROM OLYMPICS_HISTORY 
WHERE id IS NULL OR name IS NULL OR sex IS NULL OR age IS NULL 
    OR height IS NULL OR weight IS NULL OR team IS NULL 
    OR noc IS NULL OR games IS NULL OR year IS NULL 
    OR season IS NULL OR city IS NULL OR sport IS NULL 
    OR event IS NULL OR medal IS NULL;

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


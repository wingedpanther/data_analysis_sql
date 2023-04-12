## Olympic Data Analysis
This is a repository containing queries to analyze the 120 years of Olympic history dataset available on Kaggle.

## Data
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

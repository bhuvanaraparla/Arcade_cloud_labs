# ```Movie Recommendations in BigQuery ML 2.5 ```
## Task 1
```bash
bq --location=EU mk --dataset movies
 bq load --source_format=CSV \
 --location=EU \
 --autodetect movies.movielens_ratings \
 gs://dataeng-movielens/ratings.csv
 bq load --source_format=CSV \
 --location=EU   \
 --autodetect movies.movielens_movies_raw \
 gs://dataeng-movielens/movies.csv
```
## Task 2
Open Editor Navigation menu > Big Query > New editor
```bash
SELECT
  COUNT(DISTINCT userId) numUsers,
  COUNT(DISTINCT movieId) numMovies,
  COUNT(*) totalRatings
FROM
  movies.movielens_ratings
```
Click '+'
```bash
SELECT
  *
FROM
  movies.movielens_movies_raw
WHERE
  movieId < 5
```
Click '+'
```bash
CREATE OR REPLACE TABLE
  movies.movielens_movies AS
SELECT
  * REPLACE(SPLIT(genres, "|") AS genres)
FROM
  movies.movielens_movies_raw
```
## Task 4
```bash
SELECT
  *
FROM
  ML.PREDICT(MODEL `cloud-training-prod-bucket.movies.movie_recommender`,
    (
    SELECT
      movieId,
      title,
      903 AS userId
    FROM
      `movies.movielens_movies`,
      UNNEST(genres) g
    WHERE
      g = 'Comedy' ))
ORDER BY
  predicted_rating DESC
LIMIT
  5
```
```bash
SELECT
  *
FROM
  ML.PREDICT(MODEL `cloud-training-prod-bucket.movies.movie_recommender`,
    (
    WITH
      seen AS (
      SELECT
        ARRAY_AGG(movieId) AS movies
      FROM
        movies.movielens_ratings
      WHERE
        userId = 903 )
    SELECT
      movieId,
      title,
      903 AS userId
    FROM
      movies.movielens_movies,
      UNNEST(genres) g,
      seen
    WHERE
      g = 'Comedy'
      AND movieId NOT IN UNNEST(seen.movies) ))
ORDER BY
  predicted_rating DESC
LIMIT
  5
```
## Task 5
```bash
  SELECT
    *
  FROM
    ML.PREDICT(MODEL `cloud-training-prod-bucket.movies.movie_recommender`,
      (
      WITH
        allUsers AS (
        SELECT
          DISTINCT userId
        FROM
          movies.movielens_ratings )
      SELECT
        96481 AS movieId,
        (
        SELECT
          title
        FROM
          movies.movielens_movies
        WHERE
          movieId=96481) title,
        userId
      FROM
        allUsers ))
  ORDER BY
    predicted_rating DESC
  LIMIT
    100
```


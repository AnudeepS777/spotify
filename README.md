
# Spotify Advanced SQL Project

[Advanced SQL project Spotify.docx](https://github.com/user-attachments/files/18907882/Advanced.SQL.project.Spotify.docx)


# Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using SQL. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```
**Querying the Data**
After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into easy, medium, and advanced levels to help progressively develop SQL proficiency.

**Easy Queries**
Simple data retrieval, filtering, and basic aggregations.
**Medium Queries**
More complex queries involving grouping, aggregation functions, and joins.
**Advanced Queries**
Nested subqueries, window functions, CTEs, and performance optimization.

**Practice Questions**

**Easy Level**
1. Retrieve the names of all tracks that have more than 1 billion streams.
2. List all albums along with their respective artists.
3. Get the total number of comments for tracks where licensed = TRUE.
4. Find all tracks that belong to the album type single.
5. Count the total number of tracks by each artist.

**Medium Level**
1. Calculate the average danceability of tracks in each album.
2. Find the top 5 tracks with the highest energy values.
3. List all tracks along with their views and likes where official_video = TRUE.
4. For each album, calculate the total views of all associated tracks.
5. Retrieve the track names that have been streamed on Spotify more than YouTube.

**Advanced Level**
1. Find the top 3 most-viewed tracks for each artist using window functions.
2. Write a query to find tracks where the liveness score is above the average.
3. Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.

```sql
WITH cte
AS
(SELECT 
	album,
	MAX(energy) as highest_energy,
	MIN(energy) as lowest_energery
FROM spotify
GROUP BY 1
)
SELECT 
	album,
	highest_energy - lowest_energery as energy_diff
FROM cte
ORDER BY 2 DESC
```

**Technology Stack**

> Database: PostgreSQL
> SQL Queries: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
> Tools: pgAdmin 4 (or any SQL editor), PostgreSQL (via Homebrew, Docker, or direct installation)

**How to Run the Project**

1. Install PostgreSQL and pgAdmin (if not already installed).
2. Set up the database schema and tables using the provided normalization structure.
3. Insert the sample data into the respective tables.
4. Execute SQL queries to solve the listed problems.
5. Explore query optimization techniques for large datasets.

To improve query performance, we carried out the following optimization process:

**Query Optimization Technique**

> Initial Query Performance Analysis Using EXPLAIN

We began by analyzing the performance of a query using the EXPLAIN function.
The query retrieved tracks based on the artist column, and the performance metrics were as follows:
	Execution time (E.T.): 12 ms
	Planning time (P.T.): 0.19 ms

 ```sql
EXPLAIN ANALYZE -- ET = 12.28 ms , PT = 0.19 ms
SELECT
	artist,
	track,
	views
FROM spotify
WHERE artist = 'Gorillaz' 
AND
most_played_on = 'Youtube'
ORDER BY stream DESC LIMIT 50;
```
**Index Creation on the artist Column**

> To optimize the query performance, we created an index on the artist column. This ensures faster retrieval of rows where the artist is queried.
> SQL command for creating the index:
```sql
CREATE INDEX idx_artist ON spotify_tracks(artist);
```

**Performance Analysis After Index Creation**
-- After creating the index ET = 0.172 ms, PT = 0.244 ms





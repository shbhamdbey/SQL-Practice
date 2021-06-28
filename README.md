
# SQL Notes



## Order by DESC limit 8
docker-compose up
run Mac: command + enter
auto-format shift + enter

```bash
  SELECT
  customer_id,
  inventory_id,
  rental_date
FROM dvd_rentals.rental
ORDER BY inventory_id, rental_date DESC
LIMIT 8;

```

## Count(*)
```bash
  SELECT
  COUNT(*) AS row_count
FROM dvd_rentals.film_list;


```

  ## DISTINCT
```bash
SELECT DISTINCT
  rating
FROM dvd_rentals.film_list

SELECT
  COUNT(DISTINCT category) AS unique_category_count
FROM dvd_rentals.film_list

```
  ## Group Percentage
```bash

SELECT
  rating,
  COUNT(*) AS frequency,
  COUNT(*)::NUMERIC / SUM(COUNT(*)) OVER () AS percentage
FROM dvd_rentals.film_list
GROUP BY rating
ORDER BY frequency DESC;



```
## :: Numeric(As the colum is INT to cast the coloum in numeric) , Round, Over()
```bash
SELECT
  rating,
  COUNT(*) AS frequency,
  ROUND(
    100 * COUNT(*)::NUMERIC / SUM(COUNT(*)) OVER (),
    2
  ) AS percentage
FROM dvd_rentals.film_list
GROUP BY rating
ORDER BY frequency DESC;


```

## Data inspection
```bash
SELECT *
FROM health.user_logs
LIMIT 10;


```

## Record Count
```bash
SELECT COUNT(*)
FROM health.user_logs;

```
## Unique coloum Count
```bash
SELECT COUNT(DISTINCT id)
FROM health.user_logs;

```

## Individual Column Distributions check Frequnecy of DISTINCT
```bash
SELECT
  measure_value,
  COUNT(*) AS frequency
FROM health.user_logs
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;

```
## Handling Duplicate 1) Remove 2) Recreating 3) Identify exactly which rows 4) Simply ignore
```bash
   record count : SELECT COUNT(*) FROM health.user_logs;
   DISTINCT Value: SELECT DISTINCT * FROM health.user_logs;


```
## Count(*)
```bash
  SELECT
  COUNT(*) AS row_count
FROM dvd_rentals.film_list;


```
## Unique coloum Count
```bash
SELECT COUNT(DISTINCT id)
FROM health.user_logs;

```

## Count

Unique Coloumn count

```bash
  count(DISTINCT id)
  count(*)
```

## Percentage

Perceentage Calculation

```bash
 select
  id,
  count(*) as Frequency,
  round(
    100 * count(*) :: Numeric / Sum(count(*)) over(),
    2
  )
FROM
  health.user_logs
group by
  id
order by
  Frequency Desc
limit
  10;
```
## Duplicate DISTINCT Record Count by SubQuery

```bash
  Select count(*) from (select Distinct * from health.user_logs) as Distinct_frequency;

```
## Duplicate record Count With With clause


```bash
  With Duplicate_log as(
  Select
    distinct *
  from
    health.user_logs
)
select
  count(*)
from
  Duplicate_log;
```
## Creating Temporary Table with Deduplicated Records
Temporary tables are automagically deleted once a session is shut down
```bash
  Create TEMP Table Duplicated_user_log as
select
  Distinct *
from
  health.user_logs;
```
## Frequency Count of Duplicate Entry

```bash
  WITH groupby_counts AS (
  SELECT
    id,
    log_date,
    measure,
    measure_value,
    systolic,
    diastolic,
    COUNT(*) AS frequency
  FROM health.user_logs
  GROUP BY
    id,
    log_date,
    measure,
    measure_value,
    systolic,
    diastolic
)
SELECT *
FROM groupby_counts
WHERE frequency > 1
ORDER BY frequency DESC
limit 10;
```

  

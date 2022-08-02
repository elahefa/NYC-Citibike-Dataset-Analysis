# NYC Bike Dataset Exploration
## What is the most popular start station?
Most popular start station is "E 17 St & Broadway" by 291615 number of trips.
```
SELECT
  start_station_name,
  start_station_latitude,
  start_station_longitude,
  COUNT(*) AS num_trips
FROM
  `bigquery-public-data.new_york.citibike_trips`
GROUP BY
  1,
  2,
  3
ORDER BY
  num_trips DESC
LIMIT
  5
```
RESULT:
Row|start_station_name|num_trip|
---|------------------|--------|
1  |E 17 St & Broadway|291615
2  |Lafayette St & E 8 St|277060
3  |W 21 St & 6 Ave|275348
4  |West St & Chambers St|260911
5  |Pershing Square North|246181
## What is the most popular end station?
Most popular start station is "E 17 St & Broadway" by 307500 number of trips.
```
SELECT
  end_station_name,
  end_station_latitude,
  end_station_longitude,
  COUNT(*) AS num_trips
FROM
  `bigquery-public-data.new_york.citibike_trips`
GROUP BY
  1,
  2,
  3
ORDER BY
  num_trips DESC
LIMIT
  5
```
RESULT:
Row|end_station_name|num_trip|
---|------------------|--------|
1  |E 17 St & Broadway|307500
2  |W 21 St & 6 Ave|277831
3  |Lafayette St & E 8 St|275154
4  |West St & Chambers St|266036
5  |Broadway & E 14 St|243596
## What is the gender distribution of Citibike users?
67% of users are men and ~21% of users are women.
```
SELECT
    COUNT(IF(gender = 'male', 1, NULL)) male_count,
    (COUNT(IF(gender = 'male', 1, NULL))/COUNT(*))*100 AS male_percent,
    COUNT(IF(gender = 'female', 1, NULL)) female_count,
    (COUNT(IF(gender = 'female', 1, NULL))/COUNT(*))*100 AS female_percent,
    COUNT(IF(gender = 'male', 1, NULL))/COUNT(IF(gender = 'female', 1, NULL)) as ratio,
    COUNT(IF(gender = 'unknown', 1, NULL)) unknown_count,
    (COUNT(IF(gender = 'unknown', 1, NULL))/COUNT(*))*100 AS unknown_percent,
    COUNT(*) AS total_count
FROM
    `bigquery-public-data.new_york.citibike_trips`;
```
RESULT:
Row|male_count|	male_percent|	female_count|	female_percent|	ratio|unknown_count|unknown_percent|total_count|
---|------------------|--------|---|------------------|--------|---|------------------|----|
1  |22349314|67.07674676736431|6878784|20.64521767582653|3.2490210479061417|4090921|12.278035556809161|33319019

## What is the trip duration distribution of Citibike trips?
Average bike trip duration is 16 minutes.
```
SELECT AVG(tripduration)/60 AS average_trip_duration
FROM `bigquery-public-data.new_york_citibike.citibike_trips`;
```
RESULT:
Row|average_trip_duration|
---|---------------------|
1  |16.041516425648158|
## Were there new bike stations introduced or removed at any point in time? What makes you think it did or didn’t?
There were no new bike stations introduced or removed. Using the query below I changed the months and years of timestamp and checked if it gives me any start_station_id. I assumed that if there is any new bike station, people could use that station therefore there should be a start_station_id from that station.
```
SELECT
    start_station_id
FROM
    `bigquery-public-data.new_york.citibike_trips`
WHERE (starttime BETWEEN '2013-07-01 00:00:00' AND '2013-07-31 23:59:59') AND NOT EXISTS (SELECT
    start_station_id
FROM
    `bigquery-public-data.new_york.citibike_trips`
WHERE
   starttime BETWEEN '2013-08-01 00:00:00' AND '2013-08-31 23:59:59')

```

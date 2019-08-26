# template-activity-03


# Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in jupyter notebooks.

- We will be using the Bay Area Bike Share Trips Data
  (https://cloud.google.com/bigquery/public-data/bay-bike-share). 

#### Problem Statement
- You're a data scientist at Ford GoBike (https://www.fordgobike.com/), the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. What deals do you
  offer though? Currently, your company has three options: a flat price for a
  single one-way trip, a day pass that allows unlimited 30-minute rides for 24
  hours and an annual membership. 

- Through this project, you will answer these questions: 
  * What are the 5 most popular trips that you would call "commuter trips"?
  * What are your recommendations for offers (justify based on your findings)?


## Assignment 03 - Querying data from the BigQuery CLI - set up 

### What is Google Cloud SDK?
- Read: https://cloud.google.com/sdk/docs/overview

- If you want to go further, https://cloud.google.com/sdk/docs/concepts has
  lots of good stuff.

### Get Going

- Install Google Cloud SDK: https://cloud.google.com/sdk/docs/

- Try BQ from the command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun last week's queries using bq command line tool (Paste your bq
   queries):

- What's the size of this dataset? (i.e., how many trips)

```sql
SELECT count(*) as dataset_size 
FROM `bigquery-public-data.san_francisco.bikeshare_status`
```

```
+--------------+
| dataset_size |
+--------------+
|    107501619 |
+--------------+
```
- What is the earliest start time and latest end time for a trip?

```sql
'SELECT min(time), max(time) 
FROM `bigquery-public-data.san_francisco.bikeshare_status`'
```

```
 +---------------------+---------------------+
|         f0_         |         f1_         |
+---------------------+---------------------+
| 2013-08-29 12:06:01 | 2016-08-31 23:58:59 |
+---------------------+---------------------+
```

- How many bikes are there?
```sql
SELECT  SUM( DISTINCT total_bikes) FROM `deep-freehold-240002.bike_trip_data.total_bikes` 
```
```
+-------------+
| total_bikes |
+-------------+
|         541 |
+-------------+
```


### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: 
What are the most popular trips during AM (7-10 AM) and PM (4-7 PM)

- Question 2: 
Identify the most popular "commuter" trips during AM (7-10 AM) 


- Question 3: 
Identify the most popular "commuter" trips during PM (4-7 PM) 

Where are some of the low ridership commuter trips during AM and PM


- Question 4:
What is the average time duration of a bike trip for "commuters"

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1:
What are the most popular trips during AM (7-10 AM) and PM (4-7 PM)

* Answer:
```
(7-10AM)
+-----------------------------------------------+-----------------------------------------------+-----------+
|              start_station_name               |               end_station_name                | trip_freq |
+-----------------------------------------------+-----------------------------------------------+-----------+
| Harry Bridges Plaza (Ferry Building)          | 2nd at Townsend                               |      4702 |
| San Francisco Caltrain 2 (330 Townsend)       | Townsend at 7th                               |      4172 |
| Steuart at Market                             | 2nd at Townsend                               |      4114 |
| Harry Bridges Plaza (Ferry Building)          | Embarcadero at Sansome                        |      3707 |
| San Francisco Caltrain (Townsend at 4th)      | Temporary Transbay Terminal (Howard at Beale) |      3633 |
| San Francisco Caltrain (Townsend at 4th)      | Embarcadero at Folsom                         |      3471 |
| Market at Sansome                             | 2nd at South Park                             |      3466 |
| San Francisco Caltrain (Townsend at 4th)      | Harry Bridges Plaza (Ferry Building)          |      3199 |
| Steuart at Market                             | Embarcadero at Sansome                        |      3144 |
| San Francisco Caltrain (Townsend at 4th)      | Steuart at Market                             |      3045 |
| Mountain View Caltrain Station                | Mountain View City Hall                       |      2952 |
| Civic Center BART (7th at Market)             | Townsend at 7th                               |      2931 |
| San Francisco Caltrain (Townsend at 4th)      | Market at Sansome                             |      2860 |
| San Francisco Caltrain 2 (330 Townsend)       | Market at 10th                                |      2832 |
| San Francisco Caltrain (Townsend at 4th)      | Howard at 2nd                                 |      2782 |
| San Francisco Caltrain 2 (330 Townsend)       | 5th at Howard                                 |      2576 |
| Powell Street BART                            | San Francisco Caltrain 2 (330 Townsend)       |      2496 |
| San Francisco Caltrain 2 (330 Townsend)       | Embarcadero at Folsom                         |      2261 |
| San Francisco Caltrain 2 (330 Townsend)       | Howard at 2nd                                 |      2185 |
| Temporary Transbay Terminal (Howard at Beale) | 2nd at Townsend                               |      2101 |
| San Francisco Caltrain (Townsend at 4th)      | Davis at Jackson                              |      2043 |
| San Francisco Caltrain 2 (330 Townsend)       | Steuart at Market                             |      1941 |
| San Francisco Caltrain 2 (330 Townsend)       | Temporary Transbay Terminal (Howard at Beale) |      1902 |
| Harry Bridges Plaza (Ferry Building)          | San Francisco Caltrain (Townsend at 4th)      |      1840 |
| San Francisco Caltrain (Townsend at 4th)      | Yerba Buena Center of the Arts (3rd @ Howard) |      1816 |
+-----------------------------------------------+-----------------------------------------------+-----------+

(4-7PM)
+-----------------------------------------------+------------------------------------------+-----------+
|              start_station_name               |             end_station_name             | trip_freq |
+-----------------------------------------------+------------------------------------------+-----------+
| 2nd at Townsend                               | Harry Bridges Plaza (Ferry Building)     |      4456 |
| Embarcadero at Sansome                        | Steuart at Market                        |      4282 |
| Embarcadero at Folsom                         | San Francisco Caltrain (Townsend at 4th) |      4180 |
| 2nd at South Park                             | Market at Sansome                        |      3573 |
| Steuart at Market                             | San Francisco Caltrain (Townsend at 4th) |      3567 |
| Market at 10th                                | San Francisco Caltrain 2 (330 Townsend)  |      3521 |
| Temporary Transbay Terminal (Howard at Beale) | San Francisco Caltrain (Townsend at 4th) |      3451 |
| Townsend at 7th                               | San Francisco Caltrain 2 (330 Townsend)  |      3324 |
| Howard at 2nd                                 | San Francisco Caltrain (Townsend at 4th) |      2939 |
| Townsend at 7th                               | Civic Center BART (7th at Market)        |      2678 |
| 2nd at Townsend                               | Steuart at Market                        |      2582 |
| Townsend at 7th                               | San Francisco Caltrain (Townsend at 4th) |      2566 |
| Market at Sansome                             | San Francisco Caltrain (Townsend at 4th) |      2560 |
| San Francisco Caltrain 2 (330 Townsend)       | Powell Street BART                       |      2508 |
| Mountain View City Hall                       | Mountain View Caltrain Station           |      2418 |
| San Francisco Caltrain 2 (330 Townsend)       | Townsend at 7th                          |      2298 |
| Harry Bridges Plaza (Ferry Building)          | San Francisco Caltrain (Townsend at 4th) |      2258 |
| Steuart at Market                             | San Francisco Caltrain 2 (330 Townsend)  |      2247 |
| Santa Clara at Almaden                        | San Jose Diridon Caltrain Station        |      2192 |
| Temporary Transbay Terminal (Howard at Beale) | San Francisco Caltrain 2 (330 Townsend)  |      2148 |
| Beale at Market                               | San Francisco Caltrain (Townsend at 4th) |      2065 |
| Harry Bridges Plaza (Ferry Building)          | Embarcadero at Sansome                   |      2061 |
| Embarcadero at Folsom                         | San Francisco Caltrain 2 (330 Townsend)  |      2020 |
| Embarcadero at Sansome                        | Harry Bridges Plaza (Ferry Building)     |      2000 |
| San Francisco Caltrain (Townsend at 4th)      | Harry Bridges Plaza (Ferry Building)     |      1954 |
+-----------------------------------------------+------------------------------------------+-----------+

```
  * SQL query:
``` 
WITH trip_TIME AS
(SELECT trip_ID, start_station_name, end_station_name,  EXTRACT(hour FROM start_date) as start_hour
FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
SELECT TRIPS.start_station_name, TRIPS.end_station_name, count(TRIPS.start_station_name) as trip_freq
FROM `bigquery-public-data.san_francisco.bikeshare_trips` TRIPS
Inner JOIN trip_TIME on trip_TIME.trip_ID = TRIPS.trip_ID
WHERE trip_Time.start_hour between 7 and 10
GROUP BY start_station_name, end_station_name
ORDER BY trip_freq DESC LIMIT 25


WITH trip_TIME AS
(SELECT trip_ID, start_station_name, end_station_name,  EXTRACT(hour FROM start_date) as start_hour
FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
SELECT TRIPS.start_station_name, TRIPS.end_station_name, count(TRIPS.start_station_name) as trip_freq
FROM `bigquery-public-data.san_francisco.bikeshare_trips` TRIPS
Inner JOIN trip_TIME on trip_TIME.trip_ID = TRIPS.trip_ID
WHERE trip_Time.start_hour between 16 and 19
GROUP BY start_station_name, end_station_name
ORDER BY trip_freq DESC LIMIT 25

```

- Question 2:
What are top 25 popular "commuter" trips in PM (ending between 4-7 PM)

* Answer: End Hour (4-7 PM)
```
+-----------------------------------------------+------------------------------------------+-----------+
|              start_station_name               |             end_station_name             | trip_freq |
+-----------------------------------------------+------------------------------------------+-----------+
| 2nd at Townsend                               | Harry Bridges Plaza (Ferry Building)     |      4509 |
| Embarcadero at Folsom                         | San Francisco Caltrain (Townsend at 4th) |      4405 |
| Steuart at Market                             | San Francisco Caltrain (Townsend at 4th) |      3770 |
| Temporary Transbay Terminal (Howard at Beale) | San Francisco Caltrain (Townsend at 4th) |      3625 |
| Market at 10th                                | San Francisco Caltrain 2 (330 Townsend)  |      3563 |
| Townsend at 7th                               | San Francisco Caltrain 2 (330 Townsend)  |      3399 |
| Howard at 2nd                                 | San Francisco Caltrain (Townsend at 4th) |      3152 |
| Townsend at 7th                               | Civic Center BART (7th at Market)        |      2744 |
| Market at Sansome                             | San Francisco Caltrain (Townsend at 4th) |      2688 |
| Townsend at 7th                               | San Francisco Caltrain (Townsend at 4th) |      2581 |
| Beale at Market                               | San Francisco Caltrain (Townsend at 4th) |      2500 |
| Harry Bridges Plaza (Ferry Building)          | San Francisco Caltrain (Townsend at 4th) |      2370 |
| Steuart at Market                             | San Francisco Caltrain 2 (330 Townsend)  |      2285 |
| Temporary Transbay Terminal (Howard at Beale) | San Francisco Caltrain 2 (330 Townsend)  |      2189 |
| Embarcadero at Sansome                        | Harry Bridges Plaza (Ferry Building)     |      2091 |
| Embarcadero at Folsom                         | San Francisco Caltrain 2 (330 Townsend)  |      2050 |
| Clay at Battery                               | San Francisco Caltrain (Townsend at 4th) |      2003 |
| San Francisco Caltrain (Townsend at 4th)      | Harry Bridges Plaza (Ferry Building)     |      1997 |
| Market at 10th                                | San Francisco Caltrain (Townsend at 4th) |      1940 |
| Davis at Jackson                              | San Francisco Caltrain (Townsend at 4th) |      1916 |
| Howard at 2nd                                 | San Francisco Caltrain 2 (330 Townsend)  |      1892 |
| Market at 4th                                 | San Francisco Caltrain (Townsend at 4th) |      1854 |
| 5th at Howard                                 | San Francisco Caltrain 2 (330 Townsend)  |      1793 |
| 2nd at Folsom                                 | San Francisco Caltrain (Townsend at 4th) |      1777 |
| Yerba Buena Center of the Arts (3rd @ Howard) | San Francisco Caltrain (Townsend at 4th) |      1645 |
+-----------------------------------------------+------------------------------------------+-----------+

```

* SQL query:

```
WITH trip_TIME AS
(SELECT trip_ID, start_station_name, end_station_name,  EXTRACT(hour FROM end_date) as end_hour
FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
SELECT TRIPS.start_station_name, TRIPS.end_station_name, count(TRIPS.start_station_name) as trip_freq
FROM `bigquery-public-data.san_francisco.bikeshare_trips` TRIPS
Inner JOIN trip_TIME on trip_TIME.trip_ID = TRIPS.trip_ID
WHERE trip_Time.end_hour between 16 and 19
AND (TRIPS.end_station_name='San Francisco Caltrain (Townsend at 4th)'
OR TRIPS.end_station_name='San Francisco Caltrain 2 (330 Townsend)'
OR TRIPS.end_station_name='Harry Bridges Plaza (Ferry Building)'
OR TRIPS.end_station_name='Civic Center BART (7th at Market)')
GROUP BY start_station_name, end_station_name
ORDER BY trip_freq DESC LIMIT 25
```

- Question 3:

What are the top 25 "commuter" trips during AM (Starting between 7-10AM)
  * Answer:
```  
+-----------------------------------------------+------------------------------------------+-----------+
|              start_station_name               |             end_station_name             | trip_freq |
+-----------------------------------------------+------------------------------------------+-----------+
| San Francisco Caltrain (Townsend at 4th)      | Harry Bridges Plaza (Ferry Building)     |      3199 |
| Powell Street BART                            | San Francisco Caltrain 2 (330 Townsend)  |      2496 |
| Harry Bridges Plaza (Ferry Building)          | San Francisco Caltrain (Townsend at 4th) |      1840 |
| Embarcadero at Bryant                         | San Francisco Caltrain (Townsend at 4th) |      1708 |
| San Francisco Caltrain 2 (330 Townsend)       | Harry Bridges Plaza (Ferry Building)     |      1699 |
| Townsend at 7th                               | San Francisco Caltrain 2 (330 Townsend)  |      1646 |
| 2nd at Townsend                               | San Francisco Caltrain (Townsend at 4th) |      1377 |
| Temporary Transbay Terminal (Howard at Beale) | San Francisco Caltrain (Townsend at 4th) |      1296 |
| Townsend at 7th                               | San Francisco Caltrain (Townsend at 4th) |      1274 |
| 2nd at Townsend                               | Harry Bridges Plaza (Ferry Building)     |      1215 |
| Steuart at Market                             | San Francisco Caltrain (Townsend at 4th) |      1208 |
| Market at 4th                                 | San Francisco Caltrain (Townsend at 4th) |      1119 |
| Market at 10th                                | San Francisco Caltrain (Townsend at 4th) |      1040 |
| Grant Avenue at Columbus Avenue               | San Francisco Caltrain (Townsend at 4th) |       978 |
| 2nd at South Park                             | San Francisco Caltrain (Townsend at 4th) |       962 |
| 2nd at Folsom                                 | San Francisco Caltrain (Townsend at 4th) |       932 |
| Powell Street BART                            | San Francisco Caltrain (Townsend at 4th) |       931 |
| Market at 10th                                | San Francisco Caltrain 2 (330 Townsend)  |       915 |
| Embarcadero at Sansome                        | Harry Bridges Plaza (Ferry Building)     |       887 |
| Embarcadero at Folsom                         | San Francisco Caltrain (Townsend at 4th) |       830 |
| Embarcadero at Bryant                         | Harry Bridges Plaza (Ferry Building)     |       790 |
| Harry Bridges Plaza (Ferry Building)          | San Francisco Caltrain 2 (330 Townsend)  |       758 |
| Temporary Transbay Terminal (Howard at Beale) | Civic Center BART (7th at Market)        |       659 |
| Howard at 2nd                                 | San Francisco Caltrain (Townsend at 4th) |       651 |
| Broadway St at Battery St                     | San Francisco Caltrain (Townsend at 4th) |       638 |
+-----------------------------------------------+------------------------------------------+-----------+
 ```
 * SQL query:
 ```
 WITH trip_TIME AS
(SELECT trip_ID, start_station_name, end_station_name,  EXTRACT(hour FROM start_date) as start_hour
FROM `bigquery-public-data.san_francisco.bikeshare_trips`)
SELECT TRIPS.start_station_name, TRIPS.end_station_name, count(TRIPS.start_station_name) as trip_freq
FROM `bigquery-public-data.san_francisco.bikeshare_trips` TRIPS
Inner JOIN trip_TIME on trip_TIME.trip_ID = TRIPS.trip_ID
WHERE trip_Time.start_hour between 7 and 10
AND (TRIPS.end_station_name='San Francisco Caltrain (Townsend at 4th)'
OR TRIPS.end_station_name='San Francisco Caltrain 2 (330 Townsend)'
OR TRIPS.end_station_name='Harry Bridges Plaza (Ferry Building)'
OR TRIPS.end_station_name='Civic Center BART (7th at Market)')
GROUP BY start_station_name, end_station_name
ORDER BY trip_freq DESC LIMIT 25
```
- ...

- Question 4:
What is the average duration in minutes for "commuter" trips

  * Answer: 
 ```
+--------------------+
|    duration_avg    |
+--------------------+
| 15.696499060865888 |
+--------------------+
```
  * SQL query:
Create a view from the query:  
```
select start_station_name, duration_sec,  EXTRACT(hour FROM start_date) as start_hour from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name='San Francisco Caltrain (Townsend at 4th)'
OR start_station_name='San Francisco Caltrain 2 (330 Townsend)'
OR start_station_name='Harry Bridges Plaza (Ferry Building)'
OR start_station_name='Civic Center BART (7th at Market)'
ORDER BY duration_sec
```
```
select avg(duration_sec)/60 as duration_avg from `deep-freehold-240002.bike_trip_data.duration`
```
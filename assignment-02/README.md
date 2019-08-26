# Query Project
- In the Query Project, you will get practice with SQL while learning about Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven questions using public datasets housed in GCP. To give you experience with different ways to use those datasets, you will use the web UI (BiqQuery) and the command-line tools, and work with them in jupyter notebooks.
- We will be using the Bay Area Bike Share Trips Data (https://cloud.google.com/bigquery/public-data/bay-bike-share). 

#### Problem Statement
- You're a data scientist at Ford GoBike (https://www.fordgobike.com/), the company running Bay Area Bikeshare. You are trying to increase ridership, and you want to offer deals through the mobile app to do so. What deals do you offer though? Currently, your company has three options: a flat price for a single one-way trip, a day pass that allows unlimited 30-minute rides for 24 hours and an annual membership. 

- Through this project, you will answer these questions: 
  * What are the 5 most popular trips that you would call "commuter trips"?
  * What are your recommendations for offers (justify based on your findings)?


## Assignment 02: Querying Data with BigQuery

### What is Google Cloud?
- Read: https://cloud.google.com/docs/overview/

### Get Going

- Go to https://cloud.google.com/bigquery/
- Click on "Try it Free"
- It asks for credit card, but you get $300 free and it does not autorenew after the $300 credit is used, so go ahead (OR CHANGE THIS IF SOME SORT OF OTHER ACCESS INFO)
- Now you will see the console screen. This is where you can manage everything for GCP
- Go to the menus on the left and scroll down to BigQuery
- Now go to https://cloud.google.com/bigquery/public-data/bay-bike-share 
- Scroll down to "Go to Bay Area Bike Share Trips Dataset" (This will open a BQ working page.)


### Some initial queries
Paste your SQL query and answer the question in a sentence.

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

### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.
- Use the SQL tutorial (https://www.w3schools.com/sql/default.asp) to help you with mechanics.

- Question 1: What are the Top 10 popular stations for starting trips

```
* Answer:
+-----------------------------------------------+-----------+
|              start_station_name               | trip_freq |
+-----------------------------------------------+-----------+
| San Francisco Caltrain (Townsend at 4th)      |     72683 |
| San Francisco Caltrain 2 (330 Townsend)       |     56100 |
| Harry Bridges Plaza (Ferry Building)          |     49062 |
| Embarcadero at Sansome                        |     41137 |
| 2nd at Townsend                               |     39936 |
| Temporary Transbay Terminal (Howard at Beale) |     39200 |
| Steuart at Market                             |     38531 |
| Market at Sansome                             |     35142 |
| Townsend at 7th                               |     34894 |
| Market at 10th                                |     30209 |
+-----------------------------------------------+-----------+
```

* SQL query:
```sql
'SELECT start_station_name, count(start_station_name) as trip_freq FROM `bigquery-public-data.san_francisco.bikeshare_trips` GROUP BY start_station_name ORDER BY trip_freq DESC LIMIT 10'
```

- Question 2: What is the precentage of various subscriber types  

  * Answer:
```
+------------------------+-----------------+
| subsciber_type_percent | subscriber_type |
+------------------------+-----------------+
|     13.908328995738312 | Customer        |
|      86.09167100426168 | Subscriber      |
+------------------------+-----------------+
```  
  * SQL query:

```sql
SELECT  (count(trip_id) * 100/ (select count(*) 
FROM `bigquery-public-data.san_francisco.bikeshare_trips`)) 
as subsciber_type_percent, subscriber_type 
FROM  `bigquery-public-data.san_francisco.bikeshare_trips` 
GROUP BY subscriber_type'
```

- Question 3:
What are the top 10 "commuter trips" from Caltrain stations and Ferry Building

```
  * Answer:  
+------------------------------------------+-----------------------------------------------+-----------+
|            start_station_name            |               end_station_name                | trip_freq |
+------------------------------------------+-----------------------------------------------+-----------+
| Harry Bridges Plaza (Ferry Building)     | Embarcadero at Sansome                        |      9150 |
| San Francisco Caltrain 2 (330 Townsend)  | Townsend at 7th                               |      8508 |
| Harry Bridges Plaza (Ferry Building)     | 2nd at Townsend                               |      6888 |
| San Francisco Caltrain (Townsend at 4th) | Harry Bridges Plaza (Ferry Building)          |      6215 |
| Harry Bridges Plaza (Ferry Building)     | San Francisco Caltrain (Townsend at 4th)      |      5395 |
| San Francisco Caltrain (Townsend at 4th) | Temporary Transbay Terminal (Howard at Beale) |      5196 |
| San Francisco Caltrain 2 (330 Townsend)  | Powell Street BART                            |      4492 |
| San Francisco Caltrain (Townsend at 4th) | Embarcadero at Folsom                         |      4440 |
| San Francisco Caltrain 2 (330 Townsend)  | 5th at Howard                                 |      4433 |
| San Francisco Caltrain (Townsend at 4th) | Steuart at Market                             |      4367 |
+------------------------------------------+-----------------------------------------------+-----------+
```

* SQL query:

```sql
SELECT start_station_name, end_station_name, count(start_station_name) as trip_freq 
FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
WHERE start_station_name='San Francisco Caltrain (Townsend at 4th)' 
OR start_station_name='San Francisco Caltrain 2 (330 Townsend)' 
OR start_station_name='Harry Bridges Plaza (Ferry Building)' 
GROUP BY start_station_name, end_station_name 
ORDER BY trip_freq DESC LIMIT 10
```




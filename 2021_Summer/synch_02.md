### UCB MIDS W205 - Kevin Crook's supplement for Synchronous Session #2

We will try to follow the official slides as close as we can in class.  I will post commands here to make them easier for students to copy and paste.

#### Checklist before class and before working on assignments

Right now, this checklist has things in it we haven't covered yet, so just do what we have covered.

https://github.com/kevin-crook-ucb/ucb_w205_crook_supplement/blob/master/2021_Summer/checklist_b4_class_assignments.md

#### Project 1: Query Project

#### Google BigQuery Queries

Suggestions:
* use the Chrome browser
* use an incognito window to avoid cookie conflicts
* make you have given them a credit card and enabled billing on the project

Links:

https://cloud.google.com/bigquery/

https://cloud.google.com/bigquery/docs/

https://cloud.google.com/bigquery/public-data/

There are 2 sets of tables related to the San Francisco Bike Share: a static set and a dynamic set.  The dynamic set is the one mentioned in link above.  Since it's much easier for beginners to use the static set, that is what we will use.  If you use the dynamic set, the query results will not match the examples we give you, and can possible change with every 15 minute update.  Here are the tables we will be using:

```
bigquery-public-data.san_francisco.bikeshare_status
bigquery-public-data.san_francisco.bikeshare_stations
bigquery-public-data.san_francisco.bikeshare_trips
```

Example first query:
```sql
SELECT *
FROM `bigquery-public-data.san_francisco.bikeshare_status`
```

How many events are there?
```sql
SELECT count(*)
FROM `bigquery-public-data.san_francisco.bikeshare_status`
```

How many stations are there?
```sql
SELECT count(distinct station_id)
FROM `bigquery-public-data.san_francisco.bikeshare_status`
```

How long a time period do these data cover?
```sql
SELECT min(time), max(time)
FROM `bigquery-public-data.san_francisco.bikeshare_status`
```

How many bikes does station 90 have (hint: total bikes should be docks_available + bikes_available)?

Does this query give us the answer?
```sql
SELECT station_id, 
(docks_available + bikes_available) as total_bikes
FROM `bigquery-public-data.san_francisco.bikeshare_status`
WHERE station_id = 90
```

No, it's time dependent.  So, let's try this query:
```sql
SELECT station_id, docks_available, bikes_available, time, 
(docks_available + bikes_available) as total_bikes
FROM `bigquery-public-data.san_francisco.bikeshare_status`
WHERE station_id = 90
ORDER BY total_bikes
```

## Create our own private dataset named bike_trip_data, create our own private table named total_bikes in our private dataset, run some queries against our private table

In the Google BigQuery, you may need to first pin your project.  On the left side, next to "Explorer", click on "ADD DATA" to pin your project.

Once your project is pinned, to the right of your project name, you will see 3 dots.  Click on the dots and choose "Create Dataset" and name it "bike_trip_data".

Execute the following query.  

```sql
SELECT station_id, docks_available, bikes_available, time, 
(docks_available + bikes_available) as total_bikes
FROM `bigquery-public-data.san_francisco.bikeshare_status`
```

After the query results come back, click on "SAVE RESULTS", in the dropdown choose BigQuery Table, leave the project name, choose "bike_trip_data" for the dataset name, and name the table "total_bikes".

Using the GUI examine the new table you created going through all of the tabs.  Pay close attention to the naming and use it to create a similar queries to the ones below.

```sql
SELECT distinct (station_id), total_bikes
FROM `bike_trip_data.total_bikes`
```

```sql
SELECT distinct station_id, total_bikes
FROM `bike_trip_data.total_bikes`
WHERE station_id = 22
```
 
As an alternative, you could also save the query as a view.  A view does not use space but works the same as a table for the most part.  To save as a view, on the query editor, click the dropdown "SAVE", and choose "Save View".
 

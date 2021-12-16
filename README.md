# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

Make sure you receive the emails related to your repository! Your project feedback will be given as comment on the pull request. When you receive the feedback, you can address problems or simply comment that you have read the feedback. 
AFTER receiving and answering the feedback, merge you PR to master. Your project only counts as complete once this is done.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)
  * Answer: The size of this dataset, or the total number of trips, is 983,648. 
  * SQL query: 
  ```sql 
  #standardSQL
  SELECT COUNT(*) FROM bigquery-public-data.san_francisco.bikeshare_trips
  ```


- What is the earliest start date and time and latest end date and time for a trip? 
  * Answer: The earliest start date and time is 2013-08-29 (August 29th, 2013) 09:08:00 UTC (2:08 AM PST) and the latest end date and time is 2016-08-31 (August 31st, 2016) 23:48:00 UTC (4:48 PM PST). 
  * SQL query: 
  ```sql 
  #standardSQL
  SELECT MIN(start_date) FROM bigquery-public-data.san_francisco.bikeshare_trips
  ```
  ```sql 
  #standardSQL
  SELECT MAX(end_date) FROM bigquery-public-data.san_francisco.bikeshare_trips
  ```


- How many bikes are there?
  * Answer: There are 700 bikes.  
  * SQL query: 
  ```sql 
  #standardSQL
  SELECT COUNT(DISTINCT bike_number) FROM bigquery-public-data.san_francisco.bikeshare_trips
  ```


### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.


- Question 1: How many trips are longer than 30 minutes?
  * Answer: There are 47,337 trips that are longer than 30 minutes, which represents about 4.8% of all total trips. This indicates that most trips are relatively short-distance and bike rides are used for short-term convenience. 
  * SQL query: 
  ```sql 
  #standardSQL
  SELECT COUNT(*) FROM bigquery-public-data.san_francisco.bikeshare_trips WHERE duration_sec > 1800
  ```


- Question 2: What are the top 10 zip codes where the most trips come from?
  * Answer: The top 15 zip codes that have the most rides, in descending order, are:
  
    **Row** | **Zip Code** | **Number of Trips**
    --- | --- | --- 
    1 | 94107 | 106913
    2 | 94105 | 61232
    3 | 94133 | 46544
    4 | 94103 | 38072
    5 | 94111 | 33642
    6 | 94102 | 30222
    7 | 94109 | 19043
    8 | 95112 | 15420
    9 | nil | 15385
    10 | 94158 | 13673
    
    
    Although most of these zip codes belong to SF County, there is a 'nil' value, which represents missing data. It makes logical sense that bike rides, especially faster ones, are an option for urban residents in an area like San Francisco where having a car may not be as efficient or common. 
   * SQL query: 
      ```sql 
      #standardSQL
      SELECT zip_code, COUNT(*) FROM bigquery-public-data.san_francisco.bikeshare_trips GROUP BY zip_code ORDER BY COUNT(*) DESC LIMIT 10
      ```
  

- Question 3: How many of the trips were taken by subscribers? 
  * Answer: There are 846,839 trips that were taken by subscribers, which represents about 86% of all total trips. Subscribers would be defined as consistent customers that are loyal to the company and ride bikes fairly often, hence most benefit programs would affect them. Since the majority of trips were by subscribers, we can assume that most of these subscribers are frequent commuters commuters and live in the Bay Area. 
  * SQL query: 
  ```sql 
  #standardSQL
  SELECT COUNT(*) FROM bigquery-public-data.san_francisco.bikeshare_trips WHERE subscriber_type = "Subscriber"
  ```

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):


  * What's the size of this dataset? (i.e., how many trips)
    * Answer: The size of this dataset, or the total number of trips, is 983,648. 
    * bq query: 
        ```
        bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```
       
       
  * What is the earliest start time and latest end time for a trip?
      * Answer: The earliest start date and time is 2013-08-29 (August 29th, 2013) 09:08:00 UTC (2:08 AM PST) and the latest end date and time is 2016-08-31 (August 31st, 2016) 23:48:00 UTC (4:48 PM PST). 
      * bq query: 
        ```
        bq query --use_legacy_sql=false '
        SELECT MIN(start_date) 
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```
        ```
        bq query --use_legacy_sql=false '
        SELECT MAX(end_date) 
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```


  * How many bikes are there?
      * Answer: There are 700 bikes.  
      * bq query: 
        ```
        bq query --use_legacy_sql=false '
        SELECT COUNT(DISTINCT bike_number)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
        ```
    
    
2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
      * Answer: While there are about 404,919 trips in the morning (such that the morning bounds are defined as 5am-12pm), there are about 264,897 trips in the afternoon (such that the afternoon bounds are defined as 12pm-5pm). Therefore, there are about 140,022 more trips in the morning compared to the afternoon, which represents about a 65% difference. 
      * bq query: 
       ```
        bq query --use_legacy_sql=false '
        SELECT COUNT(*) 
        FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
        WHERE EXTRACT(HOUR FROM start_date) >= 5 and EXTRACT(HOUR FROM start_date) < 12'
       ```
       ```
        bq query --use_legacy_sql=false '
        SELECT COUNT(*) 
        FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
        WHERE EXTRACT(HOUR FROM start_date) >= 12 and EXTRACT(HOUR FROM start_date) < 17'
       ```


### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: How many trips are taken during rush hour for commuting, such that rush hour is defined as Monday-Friday, 7am-9am and 4pm-6pm?

- Question 2: What are the top 5 pairings of start and stop stations for trips for Subscribers? 

- Question 3: How many trips are taken during rush hour by subscribers specificially?

- Question 4: What is the most popular zip code for subscribers? 

- Question 5: How many bike trips are used for work (Monday-Friday) versus liesure (Saturday-Sunday) year-to-year? 

- Question 6: How many bike riders are male versus female?

- Question 7: What is the rate of growth for subscribers each year?

- Question 8: What time of day do subscribers ride the most?

- Question 9: What day of the week do subscribers ride the most?

- Question 10: Which month is most popular for bike rides? Least popular?

- Question 11: Are there more bike rides to and from stations near public transit centers like BART or CalTrain? 

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: How many trips are taken during rush hour for commuting, such that rush hour is defined as Monday-Friday, 7am-9am and 4pm-6pm?
  * Answer: There were about 560,683 total trips taken during the rush hour for commuting, such that the rush hour is defined as Monday-Friday, 7am-9am and 4pm-6pm. 
  * SQL query:
  ```sql 
  #standardSQL
   SELECT COUNT(*), 
    FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
    WHERE EXTRACT(DAYOFWEEK FROM start_date) NOT IN (1, 7) and (
    (EXTRACT(HOUR FROM start_date) >= 7 and EXTRACT(HOUR FROM start_date) <= 9) 
    OR (EXTRACT(HOUR FROM start_date) >= 16 and EXTRACT(HOUR FROM start_date) <= 18))
  ```


- Question 2: How many trips are taken during rush hour by subscribers specificially?
  * Answer: About 529,871 of the 560,683 total trips taken during the rush hour for commuting, were taken by subscribers. This is a majority as about 94.5% of all rush hour trips were taken by subscribers, which indicates that frequent commuters may be more likely to subscribe or that subscribers may be more likely to use the service for daily commutes. 
  * SQL query:
  ```sql 
  #standardSQL
  SELECT COUNT(*), 
    FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
    WHERE EXTRACT(DAYOFWEEK FROM start_date) NOT IN (1, 7) and (
    (EXTRACT(HOUR FROM start_date) >= 7 and EXTRACT(HOUR FROM start_date) <= 9) 
    OR (EXTRACT(HOUR FROM start_date) >= 16 and EXTRACT(HOUR FROM start_date) <= 18)) and (
    subscriber_type = "Subscriber")
  ```
  
  
- Question 3: What are the top 10 pairings of start and stop stations for trips for Subscribers?
  * Answer: The top 10 pairings of start and stop stations for trips for subscribers are: 
  
    **Row** | **Start Station Name** | **End Station Name** | **Trip Frequency**
    --- | --- | --- | ---
    1 | San Francisco Caltrain 2 (330 Townsend) | Townsend at 7th | 8305
    2 | 2nd at Townsend | Harry Bridges Plaza (Ferry Building) | 6931
    3 | Townsend at 7th | San Francisco Caltrain 2 (330 Townsend) | 6641
    4 | Harry Bridges Plaza (Ferry Building) | 2nd at Townsend | 6332
    5 | Embarcadero at Sansome | Steuart at Market | 6200
    6 | Embarcadero at Folsom | San Francisco Caltrain (Townsend at 4th) | 6158
    7 | Steuart at Market | 2nd at Townsend | 5758
    8 | San Francisco Caltrain (Townsend at 4th) | Harry Bridges Plaza (Ferry Building) | 5709
    9 | Temporary Transbay Terminal (Howard at Beale) | San Francisco Caltrain (Townsend at 4th) | 5699
    10 | Steuart at Market | San Francisco Caltrain (Townsend at 4th) | 5695
    
    
  * SQL query: 
  ```sql 
  #standardSQL
  SELECT start_station_name, end_station_name, 
    COUNT(*) as trip_freq 
    FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
    WHERE subscriber_type = "Subscriber"
    GROUP BY start_station_name, end_station_name 
    ORDER BY trip_freq DESC 
    LIMIT 10
  ```


- Question 4: What are the top 5 most popular zip codes for subscribers? 
  * Answer: The top 5 most popular zip codes for subscribers are:
    **Row** | **Zip Code** | **Subscriber Count** 
    --- | --- | --- 
    1 | 94107 | 104037
    2 | 94105 | 59582
    3 | 94133 | 45242
    4 | 94103 | 36517 
    5 | 94111 | 32615 
    
    
    It is notable that the top 37.4% of rides by subscribers belong to the Portrero District of SF, which is a residential neighborhood. This is an upper-middle-class family-oriented neighborhood with access to Caltrain (which makes logical sense considering the prevalence of Caltrain stations as top start and stop stations for subscribers). 
    
  * SQL query:
  ```sql 
  #standardSQL
  SELECT zip_code, COUNT(*) FROM bigquery-public-data.san_francisco.bikeshare_trips 
  WHERE subscriber_type = "Subscriber" 
  GROUP BY zip_code 
  ORDER BY COUNT(*) DESC
  ```
  
  
- Question 5: How many bike trips are used for liesure (Saturday-Sunday) versus work (Monday-Friday) year-to-year?
  * Answer: The top 5 most popular zip codes for subscribers are:
    **Row** | **Year** | **Weekend Trips** | **Weekday Trips**
    --- | --- | --- | ---
    1 | 2018 | 77602 | 366469
    2 | 2017 | 96265 | 423435
    3 | 2016 | 19359 | 191135
    4 | 2015 | 34209 | 312043
    5 | 2014 | 40309 | 286030
    6 | 2013 | 17777 | 82786 
    
    
      Although there is no distinguishable year-to-year pattern in either weekend trip or weekday trip numbers, it appears that in 2016, the number of liesure and work trips decreased, while in 2017, both were at an all-time high. There could be various historical and contexual reasons behind this shift, such as Spare the Air regulations or marketing. 
      
 
  * SQL query:
  ```sql 
  #standardSQL
  SELECT EXTRACT(YEAR FROM start_date) AS year, COUNT(*) as weekend_trips 
  FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
  WHERE EXTRACT(DAYOFWEEK FROM start_date) = 7  OR EXTRACT(DAYOFWEEK FROM start_date) = 1 
  GROUP BY year 
  ORDER BY year DESC
  ```
  
  ```sql 
  #standardSQL
  SELECT EXTRACT(YEAR FROM start_date) AS year, COUNT(*) as weekday_trips 
  FROM `bigquery-public-data.san_francisco_bikeshare.bikeshare_trips`
  WHERE EXTRACT(DAYOFWEEK FROM start_date) >=2  AND EXTRACT(DAYOFWEEK FROM start_date) <=6 
  GROUP BY year 
  ORDER BY year DESC
  ```

---

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)


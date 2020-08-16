# Project: Data Modeling with Cassandra


## Introduction
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, there is no easy way to query the data to generate the results, since the data reside in a directory of CSV files on user activity on the app.

As their data engineer, I was tasked to create an Apache Cassandra database which can be used to create queries on song play data to answer their questions.

> Apache Cassandra is a free and open-source, distributed, wide column store, NoSQL database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure.


## 1. Files in the repository
The source dataset, which consists of 30 seperate .CSV files, is located in the '/event_data' folder. The subsections below 

### 1.1 Project_1B_Project_Template.ipynb
This jupyter notebook does the following:
- Reads in all the .CSV files in '/event_data' folder and creates a single 'event_datafile_new.csv' file.
     - NOTE: The data written to the 'event_datafile_new.csv' file has a reduced number of columns and rows from the source .CSV files. The remaining data is
             sufficient to demonstrate the proposed database modelling architectures.
- Drops (if exists) and Creates the sparkify database. 
- Establishes connection with the sparkify database and gets cursor to it.  
- Creates (if not exists) all tables specific to the three different queries.
- Run's queries, transfers results into a dataframe and prints out the contents of the dataframe. 
- Drops (if exists) all the tables.
- Finally, closes the connection.

### 1.2 event_datafile_new.csv
Created by the above described jupyter notebook.



## 2. Table Design Description
Below will be a brief description of the three different 

### 2.1 Table_1 - 'query1'
The first query shall output the:
- artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4

The first table has been designed with the following variables: 
- "(sessionid int, itemInSession int, artist_name text, song_title text, song_length float, PRIMARY KEY (sessionid, itemInSession))"

The PRIMARY key consists of both the 'sessionid' and 'iteminsession' variables, this is that so that:
- we can meet the requirement that all PRIMARY keys must be unique, as any of those two variables by themselves will not satify the unique requirement.
- we can meet the requirement to query the table by those two variables only within the SELECT WHERE statement.

### 2.2 Table_2 - 'query2'
The second query shall output the:
- name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182

The second table has been designed with the following variables: 
- "(userid int, sessionid int, itemInSession int, artist_name text, song_title text, firstName text, lastName text, PRIMARY KEY ((userid, sessionid), itemInSession))"

The PRIMARY key consists of both the 'userid', 'sessionid' and 'iteminsession' variables, this is that so that:
- we can meet the requirement to query the table by the 'userid' and the 'sessionid' variables within the SELECT WHERE statement.
- we can meet the requirement to sort the song (sorted by the 'itemInSession' variable)
- we can meet the requirement that all PRIMARY keys must be unique. 
            
### 2.3 Table_3 - 'query3'
The third query shall output:
- every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

The second table has been designed with the following variables: 
- "(song_title text, userid int, firstName text, lastName text, PRIMARY KEY (song_title, userid))"

The PRIMARY key consists of both the 'song_title' and 'userid' variables, this is that so that:
- we can meet the requirement to query the table by the 'song_title' variable within the SELECT WHERE statement only.
- we can meet the requirement that all PRIMARY keys must be unique, specifically you need at addition of the 'userid' variable as there maybe a possibility of multiply users with the same first and last name that has listened to the subject song. Assuming the 'userid' is unique for each user this is a better variable to use in this query. 

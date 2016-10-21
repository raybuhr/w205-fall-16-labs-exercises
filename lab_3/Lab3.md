MIDS W205

Lab #              3        Lab Title          Defining Schema and Basic Queries with Hive and Spark

Related Module(s)  1-4      Goal               Understanding schema and query engines
Last Updated       1/25/16  Expected duration  40-60 minutes

Introduction:

In this lab we will be using the pseudo-distributed Big Data environment created in Lab 2 and
learn to process data with it. We will use SQL, a query language, to create define multiple
schema on a data set. Then, we will use 2 SQL-based processing engines, Apache Hive and
Apache Spark-SQL, to explore the data and compare execution. In this lab, we will learn about
the following:

     How do define schema on data stored in HDFS
     How to create tables with Apache Hive and Spark-SQL

     How to interactively use SQL with the Hive Command Line Interface (CLI)
     How to interactively use SQL with the Spark-SQL Command Line Interface

(CLI)

You may have previous experience with SQL. However, many SQL engines have slightly
different syntax. As such, it is useful to refer to the SQL documentation for Apache Hive, as it is

shared between Hive and Spark-SQL. In the below table you can find links to useful and
necessary resources discussed in this lab.

Resource                                                       What
                                                               Hive Language Manual
https://cwiki.apache.org/confluence/display/Hive/LanguageMa
nual                                                           Spark SQL Guide

http://spark.apache.org/docs/1.5.2/sql-programming-guide.html

Step-1. Download Data and Place In HDFS

We need some data in order to create schema and, ultimately, process. The data we'll consider is
a toy dataset regarding users and their weblogs. To download the data, do this:

    1. Launch an instance of UCB W205 Spring 2016

             a. Attach your EBS volume from Lab 2

             b. Find the volume location, by typing fdisk
             c. Mount the volume as follows: mount
                  /data

             d. Start HDFS, Hadoop Yarn and Hive: /root/start-hadoop.sh

             e. Start

             f. Change

             g. Make
                  /user/w205/lab_3

             h. Download the two datasets using wget. Type:

                           i. wget

                         ii. wget

             i. Make an HDFS folder for each data set and place them in HDFS

                 i. hdfs

               ii. hdfs

             iii. hdfs

               iv. hdfs

Step-3. Define Schema for The Data in Hive

Now that the data is in HDFS, we'd like to define schema on it. We'll start by creating and
querying a simple table. Then we'll add schema for both weblogs and users.

First, enter the Hive CLI by typing: hive

We are now in the Hive interactive environment. We can use this environment to explore and
integrate the data we've placed in HDFS. Let's start by defining a flat, undelimited schema over
our weblogs. In the CLI, type:
CREATE
(weblog
ROW
STORED
LOCATION
Now we can access the weblogs interactively. Type:
SELECT
You'll notice 10 weblogs return. We can filter out the header row with a query like this:
SELECT

However, we can't select individual fields or filter our results with very much nuance. To do
that, we need to add a more detailed schema. Define a new table in the CLI as follows:
CREATE
(datetime
user_id
session_id
product_id
referrer
ROW
FIELDS
STORED
LOCATION

Now we can select out just fields we may be interested in. For example, we can count the 50
most frequently occurring user_ids as follows:
SELECT
FROM
ORDER
LIMIT

Additionally, define a table on our user information. Create a table as follows:

CREATE
(
datetime
user_id
first_name
last_name
location
)
ROW
FIELDS
STORED
LOCATION

Exit the Hive CLI by typing: exit;

Step-3. Setup Spark, Use the SparkSQL CLI

As we explore different manifestations of processing, we'll pay special attention to Apache
Spark. Spark can process SQL both programmatically, and through a CLI similar to Hive. First,
we'll set up Spark via a script. This script both sets up Spark, but also creates simple scripts to
start and stops Hive's "Metastore," which provides a common repository of schema for multiple
processing environments.

    1. Download the setup script by running:

         wget

    2. Run the script by typing: bash

    3. Start the Hive metastore. Type: /data/start_metastore.sh

    4. Start the SparkSQL CLI. Type: /data/spark15/bin/spark-sql

    5. Check to see if the previously created tables are present. Type: show

    6. Run the aggregated query from the previous step. Compare the execution time:

         SELECT
         FROM
         ORDER
         LIMIT

    7. Convert the weblogs data to Parquet format:

         CREATE

    8. Run the aggregation on the new table and compare the execution time.

         SELECT
         FROM
         ORDER
         LIMIT

    9. Exit the CLI. Type: exit;

Submissions:

    1- List the execution time of the weblog aggregation query for Hive, SparkSQL, and
         SparkSQL on Parquet.

    2- How many jobs does Hive launch? Does SparkSQL launch jobs?

    3- Write a query which joins weblogs_parquet to user_info and counts the top 5 locations.
    List the locations.
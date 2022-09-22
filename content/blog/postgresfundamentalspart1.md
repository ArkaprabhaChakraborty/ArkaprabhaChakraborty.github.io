+++
showdate = true
title = "PostgreSQL Fundamentals Part-1"
date = 2022-09-20
url = "/blog/postgres_fundamentals_part_1"
+++

## Introduction

PostgreSQL was created in 1985 by [Micheal Stonebraker](https://en.wikipedia.org/wiki/Michael_Stonebraker). The name came from "Post"-Ingress. It is a free software which is really easy to learn. 
You can install postgresql on your system by following the [docs](https://www.postgresql.org/download/) or this [blog](https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart).
For starters we use the [university dataset](https://www.kaggle.com/datasets/ananta/student-performance-dataset?select=Department_Information.csv) from kaggle. This has a simple schema but lot's of data which is kinda useful for practicing indexing.

## Creating the database
Login to your postgresql interactive terminal using the following bash command:
```bash
sudo -u postgres psql
``` 
Before we create the database we need to create a user. We will be using the `postgres` user for this. 
```postgresql
CREATE USER universityadmin WITH PASSWORD 'password';
```
THe password part can be anything you want. It is also kinda optional but I would recommend you to use it. Also you can use ```ENCRYPTED``` keyword before your password to encrypt it.

Now we need to create a database. We can do this by running the following command in the terminal.
```postgresql
CREATE DATABASE university;
```
Now we need to grant all the privileges to the user we created earlier. We can do this by running the following command.
```postgresql
GRANT ALL PRIVILEGES ON DATABASE university TO universityadmin;
```
This will grant all the privileges to the user we created earlier. The `GRANT` command has two basic types, one which grants privileges on database objects which include tables, columns, databases, views etc, and the other one that grants membership in a role. You can also grant specific privileges to the user. You can read more about it [here](https://www.postgresql.org/docs/current/sql-grant.html). 
After creating the user and the database we need can login using the user we created provided a user with the same username exists in our system :). To logout from the postgresql terminal we can use:
```postgresql
\q
```
Then we can login using the user we created earlier.
```bash
psql -U universityadmin -d university
```
But if the UNIX user universityadmin doesn't exist this will throw an error `psql: FATAL: Peer authentication failed for user "universityadmin"`. So either we directly login to the database or we canstart a forced socket connection to the database using the following command: 
```bash
psql --host=localhost --dbname=university --username=postgres
```

## Creating the tables
After that we create the tables.
```postgresql
CREATE TABLE department (
    deptid VARCHAR(10) NOT NULL,
    deptname VARCHAR(60) NOT NULL,
    doe TIMESTAMPTZ NOT NULL
);
```
I have kept deptname of size 60. I will change this later to demonstrate the `ALTER` keyword and how to alter the size of a column in a table.  
```postgresql
CREATE TABLE employee (
    empid VARCHAR(10) NOT NULL,
    dob TIMESTAMPTZ NOT NULL,
    doj TIMESTAMPTZ NOT NULL,
    deptid VARCHAR(10) NOT NULL
);
```
    
```postgresql
CREATE TABLE student (
    id VARCHAR(15) NOT NULL,
    doa TIMESTAMPTZ NOT NULL,
    dob TIMESTAMPTZ NOT NULL,
    chdeptid VARCHAR(10) NOT NULL,
    deptid VARCHAR(10) NOT NULL
);
```
    
```postgresql
CREATE TABLE results (
    studentid VARCHAR(10) NOT NULL,
    sem VARCHAR(10) NOT NULL,
    paperid VARCHAR(10) NOT NULL,
    papername VARCHAR(100) NOT NULL,
    marks INT NOT NULL
);
```

To see all the tables that you have created you can use the following command:
```postgresql
\dt
```
This is how the output looks like:
```
           List of relations
 Schema |    Name    | Type  |  Owner   
--------+------------+-------+----------
 public | department | table | postgres
 public | employee   | table | postgres
 public | results    | table | postgres
 public | student    | table | postgres
```

## Add data to the tables
We will use the csv files from thge university dataset to add data to the tables. We can use the following command to add data to the tables:
```postgresql
COPY department from '<path to your downloaded Department_Information.csv file or txt file>' DELIMITER ',' CSV HEADER;
```
Make sure to replace the path with the complete path of the file. This command throw an error like this...
```
ERROR:  value too long for type character varying(60)
CONTEXT:  COPY department, line 26, column deptname: "Centre for Formal Design and Verification of Software (CFDVS)"
```
But fear not :), we can fix this easily by using the `ALTER` keyword. We can use the following command to change the size of the column:
```postgresql
ALTER TABLE department ALTER COLUMN deptname TYPE VARCHAR(200);
``` 
Now if we rerun the `COPY` command we will be able to add the data to the table. To verify that the data has been added we can use the following command:
```postgresql
SELECT * FROM department;
```
Your output should be something like this:
```
  deptid   |                                  deptname                                  |            doe            
-----------+----------------------------------------------------------------------------+---------------------------
 IDEPT4670 | Aerospace Engineering                                                      | 1961-05-31 05:30:00+05:30
 IDEPT5528 | Biosciences and Bioengineering                                             | 1943-06-28 06:30:00+06:30
 IDEPT3115 | Chemical Engineering                                                       | 1940-05-01 05:30:00+05:30
 IDEPT5881 | Chemistry                                                                  | 2013-06-08 05:30:00+05:30
 IDEPT4938 | Civil Engineering                                                          | 1941-10-27 06:30:00+06:30
 IDEPT1423 | Computer Science & Engineering                                             | 1941-05-03 05:30:00+05:30
 IDEPT4132 | Earth Sciences                                                             | 1982-08-19 05:30:00+05:30
 IDEPT2054 | Electrical Engineering                                                     | 1985-05-27 05:30:00+05:30
 IDEPT2357 | Energy Science and Engineering                                             | 2007-03-28 05:30:00+05:30
 IDEPT1537 | Humanities & Social Science                                                | 1953-09-14 05:30:00+05:30
```
We can repeat this for other tables as well. Simply replace the full path of your files and the table name. PS there are some errors that you may face but you can fix them easily from this point onwards I believe :). 

## Key takeaways

This maybe the shittiest schema design ever... There is literally no primary key or foreign keys in these schemas... But it is just for demonstration purposes :). In the next blog I would be normalizing/breaking/building (call it whatever you want because I'll be using some cool postgresql stuffs like arrays :P) these tables into manageable tables with proper relationships. 

## Simple queries to get you started

We have already come across three to four commands already namely CREATE, ALTER, SELECT and COPY. There are many more commands that you can use to perform various operations on the database. I will be listing some of the commands that I use frequently. You can read more about them [here](https://www.postgresql.org/docs/current/sql-commands.html).
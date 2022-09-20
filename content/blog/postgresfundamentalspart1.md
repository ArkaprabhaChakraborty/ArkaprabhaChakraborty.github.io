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


After that we create the tables.
```postgresql
CREATE TABLE department (
    deptid VARCHAR(10) PRIMARY KEY,
    deptname VARCHAR(60) NOT NULL,
    doe TIMESTAMPTZ NOT NULL,
);
```
    
```postgresql
CREATE TABLE employee (
    empid VARCHAR(10) PRIMARY KEY,
    dob TIMESTAMPTZ NOT NULL,
    doj TIMESTAMPTZ NOT NULL,
    deptid VARCHAR(10) REFERNCE department(deptid), 
)
```
    
```postgresql
CREATE TABLE student (
    id VARCHAR(10) PRIMARY KEY,
    doa TIMESTAMPTZ NOT NULL,
    dob TIMESTAMPTZ NOT NULL,
    chdeptid VARCHAR(10) REFERNCE department(deptid),
    deptid VARCHAR(10) REFERNCE department(deptid), 
)
```
    
```postgresql
CREATE TABLE results (
    studentid VARCHAR(10) REFERNCE student(id),
    sem VARCHAR(10) NOT NULL,
    paperid VARCHAR(10) NOT NULL,
    papername VARCHAR(60) NOT NULL,
    marks INT NOT NULL,
)
```
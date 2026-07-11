# run command to update your ubuntu server
sudo apt-get update
# run command to install mysql client on your server
sudo apt-get install mysql-client

# For python and related frameworks
sudo apt-get install python3
sudo apt-get install python3-flask
sudo apt-get install python3-pymysql
sudo apt-get install python3-boto3

# for running application
sudo python3 Empapp.py

## command to connect to RDS: 
---- mysql -h <<mysql instance endpoint>> -P 3306 -u <<username>> -p ---

## command to see the list out databases in your database ## 
show databases;

## command to create a database ## 
create database employee;

# To check if the database is created ## 
show databases;
This will show the list of all databases in your created DB

# Choose your created database: 
use employee;

# To create "employee" table in your "employee" database 
CREATE TABLE employee(
empid VARCHAR(20),
firstname VARCHAR(30),
lastname VARCHAR(30),
pri_skill VARCHAR(50),
location VARCHAR(50)
);
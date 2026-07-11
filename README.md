# Employee App Setup

Setup instructions for deploying and running the Employee application on an Ubuntu server with a MySQL (RDS) backend.

## 1. Update Ubuntu Server

```bash
sudo apt-get update
```

## 2. Install MySQL Client

```bash
sudo apt-get install mysql-client
```

## 3. Install Python and Required Frameworks

```bash
sudo apt-get install python3
sudo apt-get install python3-flask
sudo apt-get install python3-pymysql
sudo apt-get install python3-boto3
```

## 4. Run the Application

```bash
sudo python3 Empapp.py
```

## 5. Connect to RDS

```bash
mysql -h <mysql-instance-endpoint> -P 3306 -u <username> -p
```

Replace `<mysql-instance-endpoint>` and `<username>` with your actual RDS endpoint and MySQL username. You'll be prompted for the password.

## 6. Database Commands

### List existing databases

```sql
show databases;
```

### Create a new database

```sql
create database employee;
```

### Verify the database was created

```sql
show databases;
```

This will display the list of all databases, including the one you just created.

### Select the database to use

```sql
use employee;
```

### Create the `employee` table

```sql
CREATE TABLE employee (
    empid VARCHAR(20),
    firstname VARCHAR(30),
    lastname VARCHAR(30),
    pri_skill VARCHAR(50),
    location VARCHAR(50)
);
```
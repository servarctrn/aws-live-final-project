# Employee App Setup

Setup instructions for deploying and running the Employee application on an Ubuntu server with a MySQL (RDS) backend.

## Prerequisites

Before you begin, make sure you have the following in place:

- **Ubuntu server (EC2 instance)** — up and running, with SSH access configured.
- **AWS RDS MySQL instance** — created and available, with:
  - The **endpoint** (hostname) noted down
  - A **master username and password** set
  - **Port 3306** open (default MySQL port)
- **Security groups configured**:
  - EC2 instance's security group allows outbound traffic to RDS on port 3306
  - RDS instance's security group allows inbound traffic from the EC2 instance's security group (or its IP) on port 3306
- **Same VPC (or VPC peering)** — EC2 and RDS instances should be able to reach each other over the network
- **Python 3** compatible environment (will be installed below if not already present)
- **Sudo/root access** on the Ubuntu server to install packages
- **Application source code** — `Empapp.py` and any related files already present on the server (e.g. cloned from this repo)

> 💡 Tip: You can test connectivity from the EC2 instance to RDS before installing anything with:
> ```bash
> telnet <mysql-instance-endpoint> 3306
> ```
> If this hangs or fails, double-check your security group rules before proceeding.

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
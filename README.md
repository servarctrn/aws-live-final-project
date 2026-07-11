# Employee App Setup — Final Project

Setup instructions for deploying and running the Employee application on an Ubuntu EC2 instance, with a MySQL RDS backend and S3 for file storage.

## Prerequisites

Before you begin, make sure you have the following in place:

- **AWS account** with permissions to create EC2, RDS, S3, and IAM resources
- **Ubuntu EC2 instance** — provisioned and running, with SSH access configured
- **IAM role with S3 access** (e.g., `PutObject`, `GetObject` permissions) — created and **attached to your EC2 instance**, so the app can push uploads to S3 without hardcoded credentials
- **Security groups configured**:
  - EC2 instance's security group allows outbound traffic to RDS on port 3306
  - RDS instance's security group allows inbound traffic from the EC2 instance's security group (or its IP) on port 3306
- **Same VPC (or VPC peering)** — EC2 and RDS instances should be able to reach each other over the network
- **Same AWS region** — EC2, RDS, and S3 resources should all be provisioned in the same region
- **Git** installed on your EC2 instance (to clone the project repository)
- **Sudo/root access** on the Ubuntu server to install packages

> 💡 Tip: You can test connectivity from the EC2 instance to RDS before installing anything with:
> ```bash
> telnet <mysql-instance-endpoint> 3306
> ```
> If this hangs or fails, double-check your security group rules before proceeding.

---

## Step 1: Provision an Ubuntu EC2 Instance

SSH into your server and run the following commands.

**Update and upgrade the server:**

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

**Install the MySQL client:**

```bash
sudo apt-get install mysql-client -y
```

**Install Python and related frameworks:**

```bash
sudo apt-get install python3 -y
sudo apt-get install python3-flask -y
sudo apt-get install python3-pymysql -y
sudo apt-get install python3-boto3 -y
```

## Step 2: Clone the Project Repository

Clone the Python and HTML code from the GitHub repository:

```bash
git clone https://github.com/servarctrn/aws-final-project.git
```

## Step 3: Navigate to the Project Directory

```bash
cd aws-final-project
ls -l
```

You should see the contents of the directory, matching what's shown on the GitHub repo UI.

## Step 4: Configure the Application

Open `config.py` in the vi editor:

```bash
vi config.py
```

Press `i` to enter insert mode, and update the following values:

```python
customhost = "rds-endpoint"
customuser = "database username"
custompass = "database password"
customdb = "employee"
custombucket = "s3-bucket-name"
customregion = "region where your s3-bucket is located"
```

Use the **arrow keys** (up, down, left, right) to navigate the file. Once you've made your changes, save and exit by pressing `Esc` then typing `SHIFT + ZZ`.

## Step 5: Create an RDS Instance

Follow the AWS console steps to create your RDS instance. Make sure:

- The **DB identifier** is set to `employee`
- You choose your own **username** and **password** (and note them down for `config.py`)

Once the RDS instance is created, connect to it from your EC2 server:

```bash
mysql -h <mysql-instance-endpoint> -P 3306 -u <username> -p
```

After a successful connection, set up the `employee` database and table.

**Create the database:**

```sql
create database employee;
```

**Select the database:**

```sql
use employee;
```

**Create the table:**

```sql
create table employee (
  empid varchar(20),
  fname varchar(20),
  lname varchar(20),
  pri_skill varchar(20),
  location varchar(20)
);
```

## Step 6: Create an S3 Bucket

Create an S3 bucket to store uploaded files. Give it a unique name, and make sure it's created **in the same region** as your other resources (EC2, RDS).

## Step 7: Run the Application

Go back to your EC2 server and navigate to the project directory:

```bash
cd aws-final-project
sudo python3 EmpApp.py
```

Open a browser and navigate to your instance's IP address to confirm the application is running.

If everything works as expected, **take an AMI (image/template) of your server** — you'll need it for the next phase of the project.
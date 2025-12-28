# 🗄️ Amazon RDS (Relational Database Service) 

Amazon RDS is a **fully managed relational database service** that simplifies database setup, operations, scaling, backups, and high availability in the AWS Cloud.

---

## 📌 What is Amazon RDS?

Amazon RDS allows you to create, operate, and scale relational databases in the cloud without managing infrastructure.

AWS manages:

* Hardware provisioning
* Database setup
* Patching & updates
* Backups
* Failover & recovery

You focus only on application development.

---

## 🚀 Why Use Amazon RDS?

### ✅ Key Benefits

* Fully managed service
* Automated backups & recovery
* High availability (Multi-AZ)
* Easy scalability
* Secure by default
* Deep AWS integration

---

## 🗂️ Databases Supported by RDS

| Engine        | Description                  |
| ------------- | ---------------------------- |
| MySQL         | Open-source relational DB    |
| PostgreSQL    | Advanced open-source DB      |
| MariaDB       | MySQL compatible             |
| Oracle        | Enterprise-grade DB          |
| SQL Server    | Microsoft enterprise DB      |
| Amazon Aurora | AWS-optimized MySQL/Postgres |

---

## ⚙️ Core RDS Components

### 🧩 1. DB Instance

A managed database server that contains:

* CPU
* RAM
* Storage
* Database engine

---

### 🧠 2. DB Engine

The software running on the DB instance.

Examples:

* MySQL
* PostgreSQL
* MariaDB
* Oracle
* SQL Server

---

### 🖥️ 3. DB Instance Class

Defines compute power.

| Class        | Usage                 |
| ------------ | --------------------- |
| db.t3.micro  | Small / testing       |
| db.m5.large  | Production            |
| db.r5.xlarge | High memory workloads |

---

### 💽 4. Storage Types

| Type      | Description              |
| --------- | ------------------------ |
| gp3       | General purpose SSD      |
| io1 / io2 | High IOPS workloads      |
| Magnetic  | Legacy (not recommended) |

Supports **auto-scaling storage**.

---

## 🔐 Security in RDS

* Runs inside **VPC**
* Controlled using **Security Groups**
* Encrypted using **AWS KMS**
* IAM authentication supported
* No public access recommended

---

## 📈 High Availability (Multi-AZ)

* Synchronous replication
* Standby instance in another AZ
* Automatic failover
* Zero manual intervention

---

## 🔄 Read Replicas

Used to:

* Improve read performance
* Offload read queries
* Scale read-heavy workloads

Supported by:

* MySQL
* PostgreSQL
* MariaDB
* Aurora

---

## 🔄 Backup & Recovery

### Automated Backups

* Daily snapshots
* Point-in-time recovery

### Manual Snapshots

* User-controlled
* Persist until deleted

---

# 🧪 LAB 1 – Create MySQL Database Using EC2 (Manual Setup)

### 🎯 Goal

Launch EC2 → Install MySQL → Create DB & Table.

---

## Step 1: Launch EC2

* OS: Ubuntu / Amazon Linux
* Instance: t2.micro
* Allow SSH (22)

---

## Step 2: Install MySQL

### Ubuntu:

```bash
sudo apt update
sudo apt install mysql-server -y
```

### Amazon Linux:

```bash
sudo yum install mysql -y
```

---

## Step 3: Login to MySQL

```bash
sudo mysql
```

---

## Step 4: Create Database & Table

```sql
CREATE DATABASE mydb;
USE mydb;

CREATE TABLE students (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50),
  course VARCHAR(50)
);
```

---

## Step 5: Insert Data

```sql
INSERT INTO students (name, course)
VALUES
('Rahul', 'DevOps'),
('Ananya', 'Cloud'),
('John', 'Security');
```

---

## Step 6: Verify Data

```sql
SELECT * FROM students;
```

---

# 🧪 LAB 2 – Create Amazon RDS Database

## Step 1: Create Database

1. Go to **RDS → Databases → Create database**
2. Choose **Standard create**
3. Engine: MySQL / Aurora
4. Template: Free tier / Dev/Test
5. Set username & password

---

## Step 2: Configure Instance

* Instance class: `db.t3.micro`
* Storage: gp3
* Enable autoscaling (optional)

---

## Step 3: Connectivity

* Select VPC
* Public access: ❌ No
* Security Group: Allow port 3306 from EC2 SG

---

## Step 4: Create Database

Wait until status becomes **Available**.

---

# 🧪 LAB 3 – Connect EC2 to RDS

### Install MySQL Client

#### Amazon Linux 2

```bash
sudo yum install mariadb105 -y
```

#### Amazon Linux 2023

```bash
sudo dnf install mysql-community-client -y
```

---

### Connect to RDS

```bash
mysql -h <RDS-ENDPOINT> -u admin -p
```

---

### Test Connection

```sql
SHOW DATABASES;
```

---

# 🧪 LAB 4 – RDS Backup & Restore

### Create Snapshot

1. Go to RDS → Databases
2. Select DB → Actions → Take Snapshot

### Restore from Snapshot

1. Select snapshot
2. Restore Snapshot
3. New DB is created

---

# 🧪 LAB 5 – Multi-AZ Deployment

* Enable Multi-AZ
* AWS creates standby DB
* Automatic failover occurs during failure

---

# 🧪 LAB 6 – Read Replica

* Create read replica
* Use for read-heavy queries
* Reduce primary DB load

---

# 📊 Monitoring & Performance

### Monitoring Tools:

* CloudWatch Metrics
* Enhanced Monitoring
* Performance Insights

---

## 🔍 Key Metrics

* CPUUtilization
* FreeStorageSpace
* DatabaseConnections
* ReadIOPS / WriteIOPS

---

# 🧠 RDS Interview Questions

### 1️⃣ What is Amazon RDS?

Managed relational database service.

### 2️⃣ Difference between RDS and EC2 DB?

RDS is managed; EC2 requires manual DB management.

### 3️⃣ What is Multi-AZ?

High availability setup with automatic failover.

### 4️⃣ What is a Read Replica?

Read-only copy for scaling reads.

### 5️⃣ Does RDS support auto-scaling?

Yes (storage & read replicas).

### 6️⃣ Can RDS be accessed publicly?

Yes, but not recommended.

### 7️⃣ What is the difference between snapshot & backup?

Snapshot is manual; backup is automatic.

### 8️⃣ What is Aurora?

AWS-optimized relational database.

### 9️⃣ How is RDS secured?

VPC, Security Groups, KMS encryption.

### 🔟 Can we stop an RDS instance?

Yes (except some engines).

---

# ✅ Final Summary

✔ Fully managed relational database
✔ Highly available & secure
✔ Supports multiple engines
✔ Easy backup & recovery
✔ Scalable & reliable

---



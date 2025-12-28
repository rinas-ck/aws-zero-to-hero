# 🗄️ AWS DMS (Database Migration Service) 

Amazon Database Migration Service (AWS DMS) helps you migrate databases quickly and securely with **minimal downtime**. It supports both **homogeneous** and **heterogeneous** migrations.

---

## 📘 What is AWS DMS?

AWS DMS is a fully managed service that helps you migrate databases between:

* On-premises → AWS
* AWS → AWS
* AWS → On-premises
* Between different database engines

---

## ⭐ Why Use AWS DMS?

✅ Minimal downtime
✅ Secure and reliable
✅ Supports continuous replication
✅ Easy to configure
✅ Cost-effective
✅ Works with most major database engines

---

## 🧠 Supported Databases

* MySQL
* PostgreSQL
* MariaDB
* Oracle
* SQL Server
* Amazon Aurora
* Amazon Redshift

---

## 🔁 How AWS DMS Works (Simple Flow)

```
Source DB → DMS Replication Instance → Target DB
```

Steps:

1. Create Replication Instance
2. Configure Source Endpoint
3. Configure Target Endpoint
4. Create Migration Task
5. DMS migrates data

---

## 🔄 Migration Types

| Type            | Description                         |
| --------------- | ----------------------------------- |
| Full Load       | Migrates existing data only         |
| Full Load + CDC | Migrates existing + ongoing changes |
| CDC Only        | Replicates changes only             |

---

## 🟦 Types of Migration

### 🔹 Homogeneous Migration

Same database engines
Example: MySQL → MySQL

### 🔹 Heterogeneous Migration

Different engines
Example: Oracle → PostgreSQL
(Uses AWS SCT)

---

## 🔐 Security in AWS DMS

* Encryption at rest (KMS)
* Encryption in transit (TLS)
* IAM-based access
* VPC Security Groups
* Private subnet deployment

---

## 🎯 Common Use Cases

* On-prem to AWS migration
* Database modernization
* Cross-region replication
* Backup & analytics sync
* Zero-downtime migrations

---

# 🧪 AWS DMS LAB — End-to-End Migration (MySQL → MySQL)

---

## 🎯 Objective

Migrate data from a **Source MySQL DB** to a **Target MySQL DB** using AWS DMS.

---

## 🏗️ Architecture

```
EC2 (Source)
     ↓
 Source RDS (MySQL)
     ↓
   AWS DMS
     ↓
 Target RDS (MySQL)
     ↓
 EC2 (Destination)
```

---

## ✅ Prerequisites

* AWS Account
* VPC + Subnets
* Two EC2 instances
* Two RDS MySQL databases
* Security groups configured

---

## 🔐 Step 1: Security Group Configuration

### Allow inbound on port **3306**:

| Security Group | Allow From         |
| -------------- | ------------------ |
| Source DB      | Source EC2 + DMS   |
| Target DB      | Target EC2 + DMS   |
| DMS SG         | Source & Target DB |

---

## 🗄️ Step 2: Create Source & Target RDS

Create **two MySQL RDS instances**:

* Source RDS
* Target RDS

Note the **endpoints**.

---

## 💻 Step 3: Launch EC2 Instances

### Source EC2

```bash
sudo yum update -y
sudo yum install -y mariadb-server mariadb
```

### Target EC2

```bash
sudo yum install -y mariadb
```

---

## 🧪 Step 4: Create Sample Database (Source)

```sql
CREATE DATABASE dms_demo;
USE dms_demo;

CREATE TABLE employees (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  role VARCHAR(100),
  salary DECIMAL(10,2)
);

INSERT INTO employees VALUES
(1,'Alice','Engineer',70000),
(2,'Bob','Analyst',60000),
(3,'Charlie','Manager',90000);
```

Verify:

```sql
SELECT * FROM employees;
```

---

## 🔁 Step 5: Create DMS Replication Instance

Go to **AWS DMS → Replication Instances**

* Instance class: `dms.t3.medium`
* Multi-AZ: ❌ No
* VPC: same as RDS
* Security group: DMS SG

Wait until **Available**.

---

## 🔗 Step 6: Create Endpoints

### Source Endpoint

* Engine: MySQL
* Endpoint type: Source
* Provide DB credentials

### Target Endpoint

* Engine: MySQL
* Endpoint type: Target

✔ Test connection for both.

---

## 🔄 Step 7: Create Migration Task

### Task Settings:

* Migration type: **Full Load**
* Source: Source Endpoint
* Target: Target Endpoint

### Table Mapping:

```json
{
  "rules": [
    {
      "rule-type": "selection",
      "rule-id": "1",
      "rule-name": "include",
      "object-locator": {
        "schema-name": "dms_demo",
        "table-name": "%"
      },
      "rule-action": "include"
    }
  ]
}
```

Start task.

---

## ✅ Step 8: Verify Migration

On Target EC2:

```bash
mysql -h <target-endpoint> -u <user> -p
USE dms_demo;
SELECT * FROM employees;
```

✔ Data successfully migrated.

---

## 🧹 Cleanup (Important)

Delete:

* DMS Replication Instance
* Endpoints
* RDS Instances
* EC2 Instances

---

# 🎯 AWS DMS Interview Questions & Answers

### 1️⃣ What is AWS DMS?

AWS service used to migrate databases with minimal downtime.

---

### 2️⃣ What databases does DMS support?

MySQL, PostgreSQL, Oracle, SQL Server, Aurora, etc.

---

### 3️⃣ What is CDC?

Change Data Capture – replicates ongoing changes.

---

### 4️⃣ What is a replication instance?

Server that performs data migration.

---

### 5️⃣ Difference between Full Load and CDC?

* Full Load → initial data
* CDC → ongoing changes

---

### 6️⃣ Can DMS migrate between different engines?

Yes (heterogeneous migration).

---

### 7️⃣ What security does DMS provide?

IAM, KMS encryption, TLS, VPC security groups.

---

### 8️⃣ What happens if DMS fails?

Task can be restarted; data consistency maintained.

---

### 9️⃣ Can DMS migrate large databases?

Yes, supports TB-scale databases.

---

### 🔟 When should you use DMS?

When migrating databases with minimal downtime.

---

## ✅ Final Summary

✔ Fully managed migration
✔ Supports many DB engines
✔ Minimal downtime
✔ Secure & scalable
✔ Ideal for cloud migrations

---



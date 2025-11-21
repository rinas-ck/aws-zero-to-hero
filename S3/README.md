# ☁️ Amazon S3 (Simple Storage Service)

Amazon S3 is a secure, scalable, and highly durable object storage service used for storing files, backups, media, logs, and more.

---

## ⭐ Features

- Unlimited storage  
- Auto-scalable  
- Highly durable (11 9’s durability)  
- Max upload: **160GB (Console)**, **5TB (CLI)**  
- Supports Multipart Upload for large files  
- Global namespace (bucket names must be unique)

---

## 🏺 Bucket Types

| Type | Description |
|------|-------------|
| **General Purpose** | Normal bucket |
| **Directory Bucket** | Low-latency, performance-optimized |
| **Table Bucket** | Tabular data, analytics-optimized |

---

## 🧱 Storage Types

| Storage Type | Description |
|-------------|-------------|
| ☁️ **Object Storage – S3** | Store files, images, logs, apps |
| 📦 **Block Storage – EBS** | Used by EC2 instances |
| 📁 **File Storage – EFS** | Shared file system across EC2 |

---

# 🪣 S3 Bucket Management

### 🔁 Versioning
Prevents overwriting files with the same name.  
Types:
- **Unversioned**
- **Versioning Enabled**
- **Suspended**

---

## 🌐 Access Object via URL

1. Enable ACLs  
2. Disable “Block Public Access”  
3. Make object public  

---

# 🔒 S3 ACL (Access Control List)

| Type | Permissions |
|------|-------------|
| Bucket ACL | READ, WRITE |
| Object ACL | READ, WRITE, READ_ACP, WRITE_ACP |
| Single object | Independent ACL |

---

# 🔐 S3 Encryption

| Type | Where It Happens | Key Managed By |
|------|------------------|----------------|
| **Client-side** | Before upload | You |
| **SSE-S3** | In S3 | AWS |
| **SSE-KMS** | In S3 | KMS Keys |
| **DSSE-KMS** | In S3 | AWS KMS (Dual key) |

---

# 📜 Bucket Policy Example  
### Public Read (Not Recommended)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}

🌍 Replication (CRR & SRR)

Create source bucket

Create destination bucket

Enable versioning on both

Go to Replication

Create IAM role automatically

Select prefix to replicate

🧪 Lab – Enable Replication (Step-by-Step)

Create bucket A in region 1

Create bucket B in region 2

Enable versioning on both

Open bucket A → Replication

Add rule → choose bucket B

IAM role auto-creates

Upload file → Verify in bucket B

📦 Storage Classes & Lifecycle
📌 Storage Classes
Class	Description
🔵 S3 Standard	Frequent Access
🟡 S3 Standard-IA	Infrequent access
⚪ One Zone-IA	Cheaper IA, 1 AZ only
🧊 S3 Glacier Instant Retrieval	Archive, milliseconds retrieval
🧊 Glacier Flexible Retrieval	Minutes to hours
🧊 Glacier Deep Archive	12 hours retrieval
🔁 Lifecycle Configuration

Two actions:

Transition → Move to cheaper storage

Expiration → Delete after time

🧪 Lab – Lifecycle Rule

Open bucket → Management tab

Create lifecycle rule

Add prefix filter

Transition to IA after 30 days

Expire objects after 365 days

Save rule

⛔ Object Lock

Prevents deletion/modification.

Modes:

Governance Mode

Compliance Mode (Strict – cannot delete at all)

Legal Hold

📄 Server Access Logging

Steps:

Create destination bucket

Enable logging in source bucket (Properties → Logging)

Logs start storing in destination bucket

📢 S3 Event Notification

Triggers on:

Object Created

Object Deleted

Metadata Updated

Destinations:

SNS

SQS

Lambda

🚀 S3 Transfer Acceleration

Uses CloudFront global edge network for faster upload/download.

🧪 Lab – Make Files Public (ACL)

Upload object

Enable ACL

Disable “Block Public Access”

Make object public

Open the object URL

🧪 Lab – Bucket Policy Public Read
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}

🗃️ S3 Batch Operations

Perform actions on millions of objects:

Copy

Delete

Tag

Change ACL

Invoke Lambda

💸 S3 Requester Pays

Requester pays for:

Download

Data transfer

Used for public datasets.

🔐 VPC Endpoint for S3

Connects S3 privately from VPC

No internet or NAT needed

🎉 Final Summary for Interviews

Must-know S3 topics:

Versioning

Encryption (SSE-S3, KMS)

Replication

Bucket Policy vs ACL

Lifecycle

Storage Classes

Event Notifications

Object Lock

Transfer Acceleration

You are fully ready 🔥🚀




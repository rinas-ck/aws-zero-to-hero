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

### **Public Read (Not Recommended)**

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



🧪 Lab – Enable Replication


Create 2 buckets in different regions


Enable versioning on both


Open Replication tab


Create replication rule


Upload file in source bucket


Check replicated object in destination bucket



📦 Storage Classes & Lifecycle
📌 Storage Classes
ClassDescription🔵 S3 StandardFrequent Access🟡 S3 Standard-IAInfrequent access⚪ One Zone-IACheaper IA, 1 AZ only🧊 S3 Glacier Instant RetrievalArchive, milliseconds retrieval🧊 Glacier FlexibleMinutes to hours🧊 Glacier Deep Archive12 hours retrieval

🔁 Lifecycle Configuration
Two actions:


Transition → Move to cheaper storage


Expiration → Delete after time



🧪 Lab – Lifecycle Rule


Open bucket → Management tab


Create lifecycle rule


Add filters


Add transition to IA


Add expiration


Save rule



🔒 Object Lock


Implements WORM (Write Once Read Many)


Prevents deletion/modification for a set period


Retention Modes


Governance Mode – Admin can override


Compliance Mode – Even admin cannot delete


Legal Hold – No expiry until removed



📄 Server Access Logging
Used to log every access request.
Steps:


Create destination bucket


Enable logging in source bucket (Properties → Logging)


Logs will be stored in destination bucket



🎯 S3 Event Notification
Triggers events when:


Object created


Object removed


Metadata changed


Destinations:


SNS


SQS


Lambda



🚀 S3 Transfer Acceleration
Uses CloudFront edge locations for faster uploads/downloads.

🧪 Lab – Make Files Public (ACL)


Upload object


Enable ACL


Disable block public access


Make object public


Access using browser URL



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
Perform bulk operations:


Copy objects


Modify ACLs


Modify tags


Trigger Lambda for each object



🖥️ EC2 via CMD – S3 Access
Download from S3:
aws s3 ls
aws s3 cp s3://mybucket/file .
aws s3 rm s3://mybucket/file


💸 S3 Requester Pays
Requester pays the download & data transfer cost.
Useful for sharing public data sets.

🔐 VPC Endpoints for S3


Private connection to S3 without using Internet


No NAT needed



🎉 Final Summary for Interview
S3 important topics:


Versioning


Encryption (SSE-S3, SSE-KMS, DSSE)


ACL vs Bucket Policy


Lifecycle rules


Replication (CRR/SRR)


Storage classes


Event notifications


Transfer Acceleration


Object Lock & Compliance mode


Requester Pays





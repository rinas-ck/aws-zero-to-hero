# ☁️ Amazon S3 (Simple Storage Service)

Amazon S3 is a secure, scalable, and durable object storage service used for storing files, media, backups, logs, and more.

---

## ⭐ Features

- Unlimited storage  
- Highly durable  
- Auto-scalable  
- Max upload: **160GB (Console)**, **5TB (CLI)**  
- Multipart upload support  

---

## 🏺 Bucket Types

| Type | Description |
|------|-------------|
| General Purpose | Default bucket |
| Directory Bucket | Fast low-latency |
| Table Bucket | Tabular/analytics data |

---

## 🧱 Storage Types

| Type | Description |
|------|-------------|
| Object Storage | Files, images, logs |
| Block Storage | EC2 EBS volumes |
| File Storage | EFS shared FS |

---

# 🪣 S3 Bucket Management

### 🔁 Versioning
- Unversioned  
- Versioning Enabled  
- Suspended  

---

## 🌐 Access Object via URL

1. Enable ACL  
2. Disable Block Public Access  
3. Make object public  

---

# 🔒 S3 ACL (Access Control List)

| ACL Type | Permissions |
|----------|-------------|
| Bucket ACL | READ / WRITE |
| Object ACL | READ / WRITE / ACP |

---

# 🔐 S3 Encryption

| Type | Happens In | Keys By |
|------|------------|---------|
| Client-side | Before upload | You |
| SSE-S3 | S3 | AWS |
| SSE-KMS | S3 | KMS |
| DSSE-KMS | S3 | KMS (dual key) |

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
```

---

# 🌍 Replication (CRR & SRR)

- Create source bucket  
- Create destination bucket  
- Enable versioning on both  
- Open Replication  
- IAM role auto-creates  
- Select prefix to replicate  

---

## 🧪 Lab – Enable Replication

1. Bucket A in region-1  
2. Bucket B in region-2  
3. Enable versioning  
4. Go to Replication  
5. Add rule → choose bucket B  
6. IAM role auto-created  
7. Upload file → appears in B  

---

# 📦 Storage Classes & Lifecycle

### 📌 Storage Classes

- S3 Standard  
- Standard IA  
- One Zone IA  
- Glacier Instant Retrieval  
- Glacier Flexible Retrieval  
- Glacier Deep Archive  

---

# 🔁 Lifecycle Rules

**Two actions:**

1. Transition  
2. Expiration  

---

## 🧪 Lab – Lifecycle Rule

1. Open bucket → Management  
2. Add rule  
3. Add prefix  
4. Transition to IA after 30 days  
5. Expire after 365 days  

---

# ⛔ Object Lock

- WORM protection  
- Governance Mode  
- Compliance Mode  
- Legal Hold  

---

# 📄 Server Access Logging

1. Create destination bucket  
2. Enable logging in source  
3. Logs stored automatically  

---

# 📢 Event Notifications

Triggered by:
- Object Created  
- Object Deleted  
- Metadata Update  

Targets:
- SNS  
- SQS  
- Lambda  

---

# 🚀 Transfer Acceleration

Uses CloudFront edge network for faster upload/download.

---

# 🧪 Lab – Public Access (ACL)

1. Upload object  
2. Enable ACL  
3. Disable BPA  
4. Make public  
5. Open object URL  

---

# 🧪 Lab – Public Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket/*"
    }
  ]
}
```

---

# 🗃️ S3 Batch Operations

- Copy  
- Delete  
- Tag  
- ACL updates  
- Lambda per object  

---

# 💸 Requester Pays

Requester pays for:
- Data download  
- Transfer costs  

---

# 🔐 VPC Endpoint for S3

- Access S3 privately  
- No internet needed  

---

# 🎯 Interview Summary

You must know:
- Versioning  
- Encryption  
- ACL vs Policy  
- Replication  
- Lifecycle  
- Storage Classes  
- Events  
- Object Lock  
- Transfer Acceleration  

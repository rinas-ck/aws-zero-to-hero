# 🪣 Amazon S3 (Simple Storage Service)

---

## ⭐ Features

| Feature | Description |
|--------|-------------|
| 📦 Unlimited Storage | Store unlimited objects |
| 📤 Auto-scalable | Grows automatically |
| ⚡ Highly Available | 99.999999999% durability |
| 📁 Max Upload Size | 160 GB (Console), 5 TB (CLI/SDK) |
| 🔀 Multipart Upload | Required for files > 5 GB |

---

## 🗄️ Storage Types

| Type | Description |
|------|-------------|
| 🔒 Block Storage | EBS |
| 🗂️ File Storage | EFS |
| 🪣 Object Storage | S3 |

---

## 🪣 Bucket Types

| Type | Description |
|------|-------------|
| 1️⃣ General Purpose | Regular storage |
| 2️⃣ Directory Bucket | Low-latency bucket (NEW) |
| 3️⃣ Table Bucket | Optimized for tabular data |

---

# 🔧 S3 Bucket Management

## 📝 Versioning

- Prevents overwriting objects  
- Maintains history  
- Modes:  
  ✔ Versioned  
  ✔ Unversioned  
  ✔ Suspended  

### 🧪 Lab — Enable Versioning
1. Create bucket  
2. Upload a file  
3. Enable Versioning  
4. Upload same file again  
5. Check object → Versions appear  

---

# 🌍 Access Object via URL

| Step | Action |
|------|--------|
| 1️⃣ | Enable ACL |
| 2️⃣ | Disable Block Public Access |
| 3️⃣ | Make Object Public |

### 🧪 Lab — Public Object Access
1. Upload any image  
2. Turn off “Block Public Access”  
3. Enable ACL  
4. Make object → Public  
5. Copy object URL → open in browser  

---

# 🧱 S3 ACL (Access Control Lists)

| Type | Permissions |
|------|-------------|
| Bucket ACL | READ, WRITE |
| Object ACL | READ |

Modes:
- 👤 User ACL  
- 📦 Bucket ACL  

---

# 🛡️ Object Lock

| Mode | Description |
|------|-------------|
| 🔒 Governance | Prevents delete (needs special permission) |
| 🧱 Compliance | Cannot delete until expiry |
| ⚖️ Legal Hold | No expiry until removed |

### 🧪 Lab — Enable Object Lock
1. Create bucket → enable “Object Lock”  
2. Upload object  
3. Apply Legal Hold  
4. Try deleting → fails  

---

# 🔐 S3 Encryption

| Type | Where it Happens | Key Managed By |
|------|------------------|----------------|
| SSE-S3 | Bucket | AWS |
| SSE-KMS | Bucket | AWS KMS |
| SSE-C | Client | You |
| DSSE-KMS | Bucket | AWS KMS |

### 🧪 Lab — Enable Bucket Encryption
1. Go to Bucket → Properties  
2. Default Encryption  
3. Choose SSE-S3 or SSE-KMS  

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


🔁 Replication (CRR / SRR)
TypeMeaning🔄 CRRCross Region Replication🔁 SRRSame Region Replication
🧪 Lab — Enable Replication


Create Source Bucket + Destination Bucket


Enable Versioning on both


Go to Replication


Create IAM role automatically


Replicate selected prefix



📦 Storage Classes & Lifecycle
Storage Classes
ClassUse Case💙 StandardFrequent access💛 Intelligent-TieringAuto-optimize cost🟧 Standard-IAInfrequent Access🟫 One-Zone IACheap infrequent❄️ GlacierArchive🧊 Deep ArchiveLowest cost long-term

🔄 Lifecycle Configuration
Two actions:


Transition → move to cheaper storage


Expiration → auto-delete objects


🧪 Lab — Lifecycle Rule


Go to Bucket → Management


Create Rule


Transition to IA after 30 days


Expire after 1 year



🔔 S3 Event Notifications
Triggers on:


PUT (create)


DELETE


Metadata changes


Destinations:


🔁 SQS


📩 SNS


🧠 Lambda


🧪 Lab — Trigger Lambda on Upload


Create Lambda


Go to S3 → “Notifications”


Configure PUT event


Upload object


Check CloudWatch logs



⚡ S3 Transfer Acceleration


Uses CloudFront global edge locations


Faster uploads worldwide


URL becomes:
bucketname.s3-accelerate.amazonaws.com


🧪 Lab


Enable “Transfer Acceleration”


Compare speed with normal upload



🚨 S3 Requester Pays


Requester pays for download + transfer, not bucket owner.


🧪 Lab


Enable “Requester Pays” in Properties


Use AWS CLI from another account to download



🌐 VPC Endpoint + S3


Access S3 privately without internet


No NAT Gateway required


Uses Gateway VPC Endpoint


Benefits:
✔ Saves cost
✔ More secure
✔ Private traffic only

💻 Useful S3 CLI Commands
aws s3 ls
aws s3 mb s3://mybucket
aws s3 cp file.txt s3://mybucket/
aws s3 rm s3://mybucket/file.txt
aws s3 rb s3://mybucket --force


🎯 Final Real-World Labs
Lab 1 — Host Static Website
✔ Create bucket
✔ Upload index.html
✔ Make public
✔ Enable static hosting
✔ Add bucket policy
Lab 2 — Create Private Secure Bucket
✔ Block Public Access
✔ Enable KMS encryption
✔ Access via IAM Role on EC2
Lab 3 — S3 Backup System
✔ Lifecycle Rules
✔ Replication
✔ Event → Lambda → SNS


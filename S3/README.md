# ☁️ Amazon S3 (Simple Storage Service)

Amazon S3 is a secure, scalable, and durable object storage service used for storing files, media, backups, logs, and more.

---

## ⭐ Features

- Unlimited storage  
- Highly durable (11 9’s)  
- Auto-scalable  
- Max upload: **160GB (Console)** / **5TB (CLI)**  
- Multipart upload  
- Global namespace (bucket names must be unique)

---

## 🏺 Bucket Types

| Type | Description |
|------|-------------|
| General Purpose | Default bucket |
| Directory Bucket | Low-latency optimized |
| Table Bucket | For analytics/tabular data |

---

## 🧱 Storage Types

| Type | Description |
|------|-------------|
| Object Storage | S3 |
| Block Storage | EBS |
| File Storage | EFS |

---

# 🪣 S3 Bucket Management

## 🔁 Versioning

Prevents overwriting & keeps multiple versions.

Modes:
- Unversioned  
- Enabled  
- Suspended  

---

## 🌐 Access Object via URL

1. Enable ACL  
2. Disable Block Public Access  
3. Make object public  

---

# 🔒 S3 ACL (Access Control List)

| Type | Permissions |
|------|-------------|
| Bucket ACL | READ / WRITE |
| Object ACL | READ / WRITE |

---

# 🔐 S3 Encryption

| Type | Where It Happens | Managed By |
|------|------------------|------------|
| Customer-side | Client | You |
| SSE-S3 | S3 | AWS |
| SSE-KMS | S3 | AWS KMS |
| DSSE-KMS | S3 | AWS KMS |

---

# 📜 Bucket Policy Example  
### ⭐ Public Read (Not Recommended)

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
- Configure replication rule  
- IAM role auto created  

---

## 🧪 Lab – Enable Replication

1. Create bucket A (region 1)  
2. Create bucket B (region 2)  
3. Enable versioning on both  
4. Bucket A → Replication  
5. Create rule → choose bucket B  
6. Upload file → appears in B  

---

# 📦 Storage Classes & Lifecycle

## ⭐ Storage Classes

- S3 Standard  
- Standard-IA  
- One Zone-IA  
- Glacier IR  
- Glacier FR  
- Glacier Deep Archive  

---

## 🔁 Lifecycle Rules

Actions:
- Transition → cheaper storage  
- Expiration → delete objects  

---

## 🧪 Lab – Lifecycle Rule

1. Bucket → Management  
2. Create lifecycle rule  
3. Add filter  
4. Transition to IA  
5. Expire after 365 days  

---

# ⛔ Object Lock

Modes:
- Governance  
- Compliance  
- Legal Hold  

---

# 📄 Server Access Logging

1. Create destination bucket  
2. Source bucket → Logging  
3. Select target  
4. Logs start generating  

---

# 📢 Event Notifications

Triggers:
- Object created  
- Object removed  
- Object metadata changed  

Destinations:
- SNS  
- SQS  
- Lambda  

---

# 🚀 Transfer Acceleration

Uses CloudFront Edge Network for FAST uploads.

---

# 🗃️ S3 Batch Operations

Used for bulk:  
- Tagging  
- Copying  
- Deleting  
- Lambda invocation  

---

# 💸 Requester Pays

Requester pays for:
- Download  
- Data transfer  

---

# 🔐 VPC Endpoint for S3

- Private access from VPC  
- No internet required  
- Uses Gateway Endpoint  

---

# 🎯 **NEW: Static Website Hosting (ADDED)**

Amazon S3 can host static websites (HTML, CSS, JS).

### Requirements:
- Bucket **must be public**  
- **Index document** must exist  
- Static hosting must be **enabled**  

### What it supports:
✔ HTML  
✔ CSS  
✔ JS  
✔ Images  
❌ No server-side code (PHP/Node)

---

## ⭐ Website Endpoint Format

```
http://bucket-name.s3-website-region.amazonaws.com
```

Example:
```
http://myweb123.s3-website-us-east-1.amazonaws.com
```

---

## 🔧 Steps to Enable Static Hosting

1. Create bucket (public name recommended)  
2. Upload `index.html` (and optional `error.html`)  
3. Go to **Properties → Static Website Hosting**  
4. Enable hosting  
5. Enter:  
   - Index document: `index.html`  
   - Error document: `error.html`  
6. Click **Save**  

---

## 🔓 Make the Site Public

Go to:  
**Permissions → Block Public Access → Turn OFF**

Then add this bucket policy:

---

## 📜 Static Website Hosting – Public Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

---

## 🧪 Lab – Host a Static Website on S3 (FULL STEPS)

1. Create bucket → name: `myweb-demo`  
2. Upload `index.html`  
3. Go to **Properties → Static Website Hosting**  
4. Enable hosting  
5. Enter `index.html` as Index file  
6. Go to **Permissions**  
7. Disable “Block Public Access”  
8. Add public bucket policy (above)  
9. Open the endpoint URL  
10. Website loads successfully 🎉  

---

# 🎉 Final Summary for Interviews

✔ Versioning  
✔ Object Lock  
✔ Encryption (SSE-S3, SSE-KMS)  
✔ Bucket Policy vs ACL  
✔ Storage Classes  
✔ Lifecycle Rules  
✔ Replication  
✔ Logging  
✔ Transfer Acceleration  
✔ Requester Pays  
✔ Static Website Hosting (IMPORTANT)  
✔ Event Notifications  





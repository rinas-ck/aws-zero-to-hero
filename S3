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

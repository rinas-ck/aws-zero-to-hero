<div align="center">

<img src="https://img.shields.io/badge/AWS-IAM%20Notes-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white"/>

<h1 style="color:#e2e8f0;">🔐 AWS IAM – Zero-to-Hero Notes</h1>

<p style="color:#94a3b8;">Complete IAM Concepts • Policies • Roles • ARNs • Labs • Cross-Account Access</p>

</div>

---

# 🏷️ What is IAM?

IAM (Identity & Access Management) is a **global** AWS service used to secure and manage:

- **Who** can access AWS  
- **What** they can access  
- **How** they access it  

---

## 🧠 Key Features

- IAM is **GLOBAL**, not region-specific  
- IAM is **FREE**  
- IAM handles **Authentication** + **Authorization**  
- Controls permissions for ALL AWS services  

---

# 🧩 IAM Components

| Component | Description |
|----------|-------------|
| 👤 **User** | Individual identity (Developer / Admin) |
| 🗂️ **Group** | Collection of users |
| 📜 **Policy** | JSON document that defines permissions |
| 🎭 **Role** | Temporary identity for AWS services |
| 🔐 **Access Keys** | For CLI/SDK login |
| 🚫 **Root User** | Full access, use ONLY for billing |

---

# 📜 IAM Policy Structure

IAM Policies are written in **JSON** format.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Sample",
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*"
    }
  ]
}
```

### ⭐ Elements Explained

| Element | Description |
|--------|-------------|
| **Version** | Policy language version |
| **Effect** | Allow / Deny |
| **Action** | What actions to allow or deny |
| **Resource** | Which AWS resources |
| **Condition** | Extra logic (IP, MFA, time, etc.) |

---

# 🆔 Amazon Resource Name (ARN)

ARN = Unique Identifier for every AWS resource.

Format:

```
arn:partition:service:region:account-id:resource
```

Example:

```
arn:aws:s3:::mybucket
arn:aws:iam::123456789012:user/Admin
```

---

# 🧱 IAM Policies Example

IAM Policies define **what actions are allowed or denied**.

---

## 🎯 Allow & Deny Access to Specific S3 Buckets

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessToBuckets",
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::bucket-677",
        "arn:aws:s3:::test-bucket-7988"
      ]
    },
    {
      "Sid": "DenyRestrictedBucket",
      "Effect": "Deny",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::test-bucket-86786"
      ]
    }
  ]
}
```

---

# 🧩 Types of IAM Policies

| Policy Type | Description |
|-------------|-------------|
| 🧑‍💻 **Identity-based Policy** | Attached to Users/Groups/Roles |
| 📦 **Resource-based Policy** | Attached to AWS resources (e.g., S3) |
| 🧾 **Inline Policy** | Not reusable, for one identity only |
| 🪪 **Managed Policy** | AWS or customer-managed reusable policies |
| ⚡ **Session Policy** | Temporary policy with STS |
| 🏢 **SCP (Service Control Policy)** | Organizational-level restrictions |

---

# 🎭 IAM Roles

Roles provide **temporary credentials** using AWS STS.

### Used by:

- EC2  
- Lambda  
- ECS  
- Cross-Account users  

---

## 🛠 Steps to Create Role

```
1. Choose AWS service (EC2 / Lambda)
2. Attach permissions (policy)
3. AWS service assumes the role automatically
```

---

## Example Trust Policy (Role Trust Relationship)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

# 🔄 Cross-Account Access

### 🎯 Goal  
Allow user in **Account A** to access resources in **Account B**.

---

## 👤 Account B (Friend Account)

```
1. Create Role → Another AWS Account
2. Enter Account A ID
3. Attach: AmazonS3ReadOnlyAccess
4. Copy Role ARN
```

Example:

```
arn:aws:iam::123456789012:role/CrossAccountAccessRole
```

---

## 👨‍💻 Account A (Your Account)

Attach this policy to your IAM user:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::123456789012:role/CrossAccountAccessRole"
    }
  ]
}
```

Then:  
**AWS Console → Switch Role**

---

# 🧪 IAM LABS (Beginner → Advanced)

---

# 🧪 LAB 1 — Create IAM User + Console Login

### Steps:
1. IAM → Users → Create User  
2. Enable console password  
3. Attach: `AmazonS3ReadOnlyAccess`  
4. Login using IAM login URL  

---

# 🧪 LAB 2 — Create IAM Group & Add Users

```
IAM → Groups → Create Group → Attach Policy → Add Users
```

Example:  
Group = `Developers`  
Policy = `AmazonEC2FullAccess`

---

# 🧪 LAB 3 — Create IAM Role for EC2

### Steps:
1. IAM → Roles → Create Role  
2. Select: **EC2**  
3. Attach: `AmazonS3ReadOnlyAccess`  
4. Launch EC2 → Attach role  
5. SSH into EC2 → run:

```bash
aws s3 ls
```

---

# 🧪 LAB 4 — Custom Inline Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::mydevbucket",
        "arn:aws:s3:::mydevbucket/*"
      ]
    }
  ]
}
```

---

# 🧪 LAB 5 — Cross-Account Role (Advanced)

### Account B:
- Create Role  
- Trust Account A  
- Attach S3 policy  

### Account A:
- Attach STS AssumeRole policy  
- Switch Role  
- Test S3 access  

---

# 🛡️ IAM Best Practices

- Enable **MFA**  
- Never use **Root User**  
- Use **Least Privilege** access  
- Rotate access keys  
- Use **Groups** instead of individual permissions  
- Use **Roles** for EC2, Lambda  

---




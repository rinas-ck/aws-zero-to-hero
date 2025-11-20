<div align="center">

<img src="https://img.shields.io/badge/AWS-IAM%20Notes-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white"/>

<h1 style="color:#e2e8f0;">🔐 IAM Notes – Zero-to-Hero</h1>

</div>

---

# 🏷️ What is IAM?

IAM (Identity & Access Management) is a global AWS security service that helps you securely manage:

- **Who** can access AWS  
- **What** they can access  
- **How** they can access  

---

## 🧠 Key Points

- IAM is **GLOBAL** (not region-specific)  
- IAM is **FREE**  
- Used for **Authentication** (login) and **Authorization** (permissions)

---

# 🧩 IAM Components

| Component | Description |
|----------|-------------|
| 🧑‍💻 **User** | Individual identity (Developer, Admin, Tester) |
| 🗂️ **Group** | Collection of IAM Users |
| 📜 **Policy** | JSON document that defines permissions |
| 🧑‍🏫 **Role** | Temporary identity used by AWS services (EC2, Lambda, Cross-Account) |
| ⛔ **Root User** | Owner account – Use ONLY for billing |

---

# 📜 IAM Policy Structure

IAM Policies are written in **JSON**.

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


---

# 🧱 IAM Policies Example

IAM Policies define **what actions are allowed or denied** for a user, group, or role.

---

## 🎯 Allow & Deny Access to Buckets

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
| 🧑‍💻 **Identity-based Policy** | Attached to Users, Groups, or Roles |
| 📦 **Resource-based Policy** | Attached to resources (e.g., S3 bucket policy) |
| 🧾 **Inline Policy** | Directly written for one user/group/role — NOT reusable |
| 🪪 **Managed Policy** | Prebuilt AWS policies or custom reusable ones |
| ⚡ **Session Policy** | Temporary permissions via STS |
| 🏢 **Organizational SCP** | Service Control Policies for AWS Organizations |

---

# 🧑‍🏫 IAM Roles

IAM Roles are temporary identities used by AWS services or external users.

### 🔥 Purpose
- Grant temporary access using STS (Secure Token Service)  
- Used for:  
  ✔ EC2 instances  
  ✔ Lambda  
  ✔ Cross-account access  

---

## 🛠 Steps to Create a Role

```md
1. Create a Role & Assign Permissions  
2. Create an Inline Policy for the Role  
```

### Example: Allow STS AssumeRole

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::123456789012:role/TempRole"
    }
  ]
}
```

---

# 🔄 Cross-Account Role Access

### 🎯 Goal  
Allow your IAM user (Account A) to access a friend's AWS Account (Account B).

---

## 👤 Friend’s Account (B)

```md
1. Create Role → Choose Another AWS Account  
2. Enter your Account ID  
3. Attach Policy (e.g., S3ReadOnlyAccess)  
4. Copy Role ARN  
```

Example Role ARN:

```
arn:aws:iam::123456789012:role/CrossAccountAccessRole
```

---

## 👨‍💻 Your Account (A)

### Attach Policy:

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

Then go to:

**AWS Console → Switch Role**

---



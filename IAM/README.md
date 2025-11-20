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




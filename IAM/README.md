AWS Identity & Access Management (IAM) – Zero to Hero Notes
<p align="center"> <img src="https://img.shields.io/badge/AWS-IAM%20Notes-232F3E?style=for-the-badge&logo=amazonaws&logoColor=gold"/> </p>


## 🔐 What is IAM?
IAM (Identity & Access Management) is a global AWS security service that helps you securely manage:

-Who can access AWS

-What they can access

-How they can access

🧠 Key Points

-IAM is GLOBAL (not region-specific)

-IAM is FREE

-Used for Authentication (login) and Authorization (permissions)


## 🧩 IAM Components

Component	                Description
👤 User	                  Individual identity (Developer, Admin, Tester)
🗂️ Group	                Collection of IAM Users
📜 Policy	                JSON document that defines permissions
🧑‍💼 Role	                  Temporary identity used by AWS services (EC2, Lambda, Cross-Account)
🛑 Root User              Owner account – Use ONLY for billing!


## 📜 IAM Policy Structure
IAM Policies are written in JSON.

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


🛑 Root User	Owner account – Use ONLY for billing!

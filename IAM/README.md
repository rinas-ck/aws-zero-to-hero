✨ IAM – Identity & Access Management (AWS)

🌩️ What is IAM?

IAM (Identity & Access Management) is a service that helps you securely control access to AWS services and resources.

-IAM is GLOBAL (not region-specific)

-Free AWS service

-Used for authentication & authorization

-Controls “Who can access What”


👥 IAM Components

Component	           Description
User               	 Human identity (developer, admin, tester)
Group	               Collection of users (e.g., DevTeam, AdminTeam)
Role	               Access given to AWS services or external users (EC2, Lambda, Cross-Account)
Policy	             JSON document that grants permissions
Identity	           Users + Groups + Roles
Root User	           Owner account (Use only for emergency)


📝 IAM Policy Structure
IAM policies are written in JSON.

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


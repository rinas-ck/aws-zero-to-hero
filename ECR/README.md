# 🐳 AWS ECR (Elastic Container Registry) 

Amazon Elastic Container Registry (ECR) is a **fully managed Docker container registry** that makes it easy to store, manage, and deploy container images securely within AWS.

---

## 📌 What is AWS ECR?

Amazon ECR is a **private container image registry** used to store, manage, and deploy Docker images.

Think of it as:

> **Docker Hub, but private, secure, and deeply integrated with AWS.**

---

## 🎯 Why Use AWS ECR?

✔ Secure storage for Docker images
✔ Deep integration with ECS, EKS, Lambda, Fargate
✔ IAM-based authentication
✔ No infrastructure to manage
✔ High availability & durability
✔ Fast image pulls inside AWS network

---

## ⭐ Key Features of AWS ECR

### 🔐 1. Secure & Private Repositories

* Images encrypted at rest
* IAM-based access control

### ⚙️ 2. Fully Managed

* No servers to maintain
* AWS manages scaling & availability

### 🚀 3. Highly Available

* Replicated across multiple Availability Zones

### 🔄 4. Image Versioning

* Multiple tags per image
* Easy rollbacks

### 🛡️ 5. Vulnerability Scanning

* Automatically scans images for security issues

### 🧹 6. Lifecycle Policies

* Automatically delete unused or old images
* Reduces storage cost

---

## 🔄 How AWS ECR Works (Simple Flow)

```
Developer → Build Docker Image
        → Tag Image
        → Push to ECR
        → ECS / EKS / Lambda pulls image
        → Application runs
```

---

## 🧩 Visual Workflow

```
[ Developer ]
     |
     v
[ Docker Build ]
     |
     v
[ Push Image to ECR ]
     |
     v
[ ECS / EKS / Lambda ]
     |
     v
[ Application Running ]
```

---

## 🧪 LAB – Push & Pull Docker Image Using AWS ECR

This lab demonstrates how to:

* Push a Docker image to ECR
* Delete it locally
* Pull it back from ECR

---

## 📌 Prerequisites

* EC2 instance (Amazon Linux preferred)
* Docker installed
* AWS CLI configured
* IAM user with ECR permissions
* One ECR repository created

---

## 🟩 Step 1 – Configure AWS CLI

```bash
aws configure
```

Enter:

* Access Key
* Secret Key
* Region (e.g., ap-south-1)
* Output format: json

---

## 🟩 Step 2 – Create ECR Repository

Go to:

```
AWS Console → ECR → Create Repository
```

Example repository name:

```
nginx-demo
```

Copy the repository URI:

```
123456789012.dkr.ecr.ap-south-1.amazonaws.com/nginx-demo
```

---

## 🟩 Step 3 – Pull NGINX Image from Docker Hub

```bash
docker pull nginx
```

Verify:

```bash
docker images
```

---

## 🟩 Step 4 – Login to ECR

```bash
aws ecr get-login-password --region ap-south-1 \
| docker login --username AWS --password-stdin 123456789012.dkr.ecr.ap-south-1.amazonaws.com
```

---

## 🟩 Step 5 – Tag the Image

```bash
docker tag nginx:latest 123456789012.dkr.ecr.ap-south-1.amazonaws.com/nginx-demo:latest
```

---

## 🟩 Step 6 – Push Image to ECR

```bash
docker push 123456789012.dkr.ecr.ap-south-1.amazonaws.com/nginx-demo:latest
```

Image is now stored in ECR.

---

## 🟩 Step 7 – Delete Local Images

```bash
docker rmi nginx:latest
docker rmi 123456789012.dkr.ecr.ap-south-1.amazonaws.com/nginx-demo:latest
```

Verify:

```bash
docker images
```

---

## 🟩 Step 8 – Pull Image Back from ECR

```bash
docker pull 123456789012.dkr.ecr.ap-south-1.amazonaws.com/nginx-demo:latest
```

Confirm:

```bash
docker images
```

---

## 🟩 Step 9 – Run the Container (Optional)

```bash
docker run -d -p 80:80 nginx
```

Open browser:

```
http://<EC2-PUBLIC-IP>
```

🎉 NGINX is now running from ECR!

---

# 🧠 AWS ECR Interview Questions & Answers

### 1️⃣ What is Amazon ECR?

Amazon ECR is a fully managed Docker container registry used to store and manage container images securely.

---

### 2️⃣ What is the difference between ECR and Docker Hub?

| ECR                 | Docker Hub        |
| ------------------- | ----------------- |
| Private & secure    | Public by default |
| IAM-based access    | Username/password |
| Integrated with AWS | External service  |

---

### 3️⃣ Is ECR regional or global?

ECR is **regional**, but supports cross-region replication.

---

### 4️⃣ How does authentication work in ECR?

Using AWS IAM credentials and `get-login-password`.

---

### 5️⃣ What is image scanning in ECR?

Automatically scans images for vulnerabilities.

---

### 6️⃣ Can ECR be used with ECS and EKS?

Yes, it integrates natively with ECS, EKS, and Lambda.

---

### 7️⃣ What is lifecycle policy?

A rule that automatically deletes old or unused images.

---

### 8️⃣ Is ECR highly available?

Yes, it is replicated across multiple AZs.

---

### 9️⃣ Can I pull images without internet?

Yes, using VPC endpoints (PrivateLink).

---

### 🔟 What happens if I delete an image?

Containers using it keep running, but new pulls fail.

---

## 🧾 Summary

✔ Secure Docker image storage
✔ Fully managed & scalable
✔ Integrates with ECS, EKS, Lambda
✔ Supports scanning & lifecycle policies
✔ Easy CI/CD integration

---



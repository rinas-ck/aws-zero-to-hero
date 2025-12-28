# 🚀 Amazon ECS (Elastic Container Service) – Complete Guide

Amazon **Elastic Container Service (ECS)** is a **fully managed container orchestration service** that helps you run, manage, and scale Docker containers on AWS **without managing servers manually**.

---

## 🌐 What is Amazon ECS?

Amazon ECS allows you to run containerized applications using Docker containers on AWS infrastructure.

You can run containers using:

* **EC2 (self-managed compute)**
* **AWS Fargate (serverless compute)**

---

## ⭐ Why Use Amazon ECS?

✔ No container orchestration management
✔ Fully managed by AWS
✔ Highly scalable and reliable
✔ Integrates with ALB, IAM, CloudWatch
✔ Secure and cost-efficient

---

## 🧩 Core ECS Components

### 🧱 1. Cluster

A logical group of compute capacity (EC2 or Fargate).

### 📦 2. Task Definition

Blueprint that defines:

* Container image
* CPU & memory
* Port mappings
* Environment variables

### 🚀 3. Task

A running instance of a task definition.

### 🧰 4. Service

Ensures a specified number of tasks are always running.

### 🖥️ 5. Container Instance

An EC2 instance running the ECS agent.

---

## 🔁 ECS Launch Types

| Launch Type | Description                     |
| ----------- | ------------------------------- |
| **EC2**     | You manage EC2 instances        |
| **Fargate** | Serverless, AWS manages compute |

---

## 🔐 Security in ECS

* IAM roles for tasks
* Security Groups
* VPC networking
* Secrets Manager integration
* Private networking with ALB

---

## 🔄 ECS Workflow (Simple)

User → Load Balancer → ECS Service → ECS Tasks → Containers

---

# 🧪 LAB 1 – Deploy ECS Using EC2 (Classic)

---

## 🎯 Objective

Deploy a containerized web application using ECS + EC2.

---

## 🧱 Step 1: Create ECS Cluster

1. Go to **ECS → Clusters**
2. Click **Create Cluster**
3. Choose **EC2 Linux + Networking**
4. Name: `ecs-cluster`
5. Create cluster

---

## 🧰 Step 2: Create Task Definition

1. Go to **Task Definitions → Create**
2. Choose **EC2**
3. Container name: `web`
4. Image: `nginx`
5. Port mapping: `80`
6. Save

---

## 🚀 Step 3: Create Service

1. Open cluster → Create service
2. Launch type: **EC2**
3. Task definition: select above
4. Desired tasks: `1`
5. Create service

---

## 🌐 Step 4: Access Application

Get EC2 public IP:

```
http://<EC2-PUBLIC-IP>
```

You should see **Nginx default page**.

---

# 🧪 LAB 2 – ECS with Fargate (Serverless)

---

## 🎯 Objective

Run containers **without managing EC2 instances**.

---

## 🧱 Step 1: Create Task Definition

* Launch type: **Fargate**
* OS: Linux
* CPU: 0.5 vCPU
* Memory: 1GB
* Image: `nginx`
* Port: 80

---

## 🧱 Step 2: Create Service

* Launch type: **Fargate**
* Network: VPC + Subnets
* Auto-assign public IP: Enabled

---

## 🌍 Step 3: Access Application

Copy **Public IP** of the task and open in browser.

---

# 🧪 LAB 3 – ECS with ALB (Production Setup)

---

## 🎯 Objective

Expose ECS services using Application Load Balancer.

---

## 🧱 Steps:

1. Create **Target Group**
2. Create **ALB**
3. Attach ALB to ECS Service
4. Listener → Forward to target group

---

## 🌐 Access App

```
http://<ALB-DNS-NAME>
```

---

# 🔐 IAM Roles in ECS

| Role                | Purpose                 |
| ------------------- | ----------------------- |
| Task Execution Role | Pull images, write logs |
| Task Role           | Access AWS services     |
| Instance Role       | Used by EC2 hosts       |

---

# 📊 Monitoring & Logging

* CloudWatch Logs
* CloudWatch Metrics
* Container Insights

---

# 🧠 ECS Interview Questions & Answers

---

### 1️⃣ What is Amazon ECS?

A fully managed container orchestration service.

---

### 2️⃣ Difference between ECS and EKS?

ECS is AWS native and simpler.
EKS uses Kubernetes.

---

### 3️⃣ What are ECS launch types?

EC2 and Fargate.

---

### 4️⃣ What is a Task Definition?

Blueprint for containers.

---

### 5️⃣ What is a Service?

Ensures desired number of tasks are running.

---

### 6️⃣ What is Fargate?

Serverless compute engine for containers.

---

### 7️⃣ Can ECS scale automatically?

Yes, using Auto Scaling policies.

---

### 8️⃣ How does ECS handle networking?

Uses VPC, ENI, Security Groups.

---

### 9️⃣ What is ECS Cluster?

Logical group of compute capacity.

---

### 🔟 ECS vs Kubernetes?

| ECS          | Kubernetes    |
| ------------ | ------------- |
| AWS native   | Open source   |
| Easier setup | Complex       |
| Less control | More flexible |

---

## 🏁 Final Summary

✔ ECS is ideal for containerized workloads
✔ Supports EC2 & Fargate
✔ Highly scalable & secure
✔ Integrated with AWS ecosystem
✔ Best choice for microservices

---


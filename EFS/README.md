# 📁 Amazon EFS (Elastic File System) 

Amazon **Elastic File System (EFS)** is a fully managed, scalable, and shared file storage service for Linux-based workloads.
It allows **multiple EC2 instances to access the same data simultaneously** using the **NFS protocol**.

---

## 🚀 What is Amazon EFS?

Amazon EFS is a **fully managed file storage service** that provides a simple, scalable, and elastic file system for use with AWS compute services such as EC2, ECS, and EKS.

It automatically scales as files are added or removed, without any need for manual capacity planning.

---

## ⭐ Why Use EFS?

✔ Shared file system across multiple EC2 instances
✔ Automatically scales up and down
✔ Highly available (Multi-AZ by default)
✔ Secure (Encryption at rest & in transit)
✔ Fully managed by AWS
✔ Pay only for what you use

---

## 📘 How EFS Works (Simple Explanation)

1. You create an **EFS file system**
2. AWS automatically creates **mount targets** in each Availability Zone
3. EC2 instances mount EFS using **NFSv4**
4. Multiple EC2 instances read/write the same files simultaneously

---

## 🧰 Common Use Cases

* 🌐 Web servers (Apache / Nginx shared content)
* 📝 WordPress shared uploads
* 📊 Big data analytics
* 🧑‍💻 Shared home directories
* 📦 ECS / EKS persistent storage

---

## 🔄 EFS vs EBS (Quick Comparison)

| Feature | EFS | EBS |
|------|------|
| Storage Type | File storage | Block storage |
| Access | Multiple EC2s | Single EC2 |
| Scaling | Automatic | Manual |
| Availability | Multi-AZ | Single AZ |
| Best For | Shared workloads | OS / Database |

---

## 🛠️ Basic Mount Commands

### Install NFS utilities

**Amazon Linux**

```bash
sudo yum install -y nfs-utils
```

**Ubuntu**

```bash
sudo apt install -y nfs-common
```

### Create mount directory

```bash
sudo mkdir /efs
```

### Mount EFS

```bash
sudo mount -t nfs4 fs-xxxx.efs.<region>.amazonaws.com:/ /efs
```

---

# 🧪 LAB – Configure AWS EFS & Mount on Two EC2 Instances

This lab demonstrates creating an EFS file system and mounting it on two EC2 instances.

---

## 🎯 Objective

✔ Create an EFS file system
✔ Launch two EC2 instances
✔ Mount the same EFS
✔ Verify shared access

---

## 🏗️ Step 1 — Create EFS File System

1. Go to **AWS Console → EFS**
2. Click **Create file system**
3. Choose **Recommended settings**
4. Click **Create**

AWS automatically creates mount targets in all AZs.

---

## 🔐 Step 2 — Configure Security Group

Allow **NFS traffic**:

| Type | Port | Source             |
| ---- | ---- | ------------------ |
| NFS  | 2049 | EC2 Security Group |

---

## 💻 Step 3 — Launch Two EC2 Instances

* Same VPC
* Same or reachable subnets
* Security group allows NFS

---

## 📌 Step 4 — Mount EFS on EC2 Instance 1

```bash
sudo mkdir /efs1
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-xxxx.efs.region.amazonaws.com:/ /efs1
```

---

## 📌 Step 5 — Mount EFS on EC2 Instance 2

```bash
sudo mkdir /efs2
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-xxxx.efs.region.amazonaws.com:/ /efs2
```

---

## 📁 Step 6 — Test Shared Access

### On Instance 1:

```bash
cd /efs1
sudo touch testfile.txt
sudo mkdir testfolder
```

### On Instance 2:

```bash
cd /efs2
ls
```

✔ You should see:

```
testfile.txt
testfolder
```

---

## 🎉 Lab Completed Successfully!

You have successfully:

* Created an EFS file system
* Mounted it on two EC2 instances
* Verified shared access

---

# 🧠 EFS Interview Questions & Answers

## 1️⃣ What is Amazon EFS?

EFS is a fully managed, scalable file storage service that allows multiple EC2 instances to access the same data simultaneously.

---

## 2️⃣ What protocol does EFS use?

EFS uses **NFSv4 (Network File System)**.

---

## 3️⃣ Can multiple EC2 instances access EFS?

Yes, multiple EC2 instances can mount and access EFS at the same time.

---

## 4️⃣ Is EFS highly available?

Yes, EFS stores data across multiple Availability Zones automatically.

---

## 5️⃣ Difference between EFS and EBS?

| Feature | EFS | EBS |
|------|------|
| Access | Multiple EC2 | Single EC2 |
| Type | File storage | Block storage |
| Scaling | Automatic | Manual |
| Availability | Multi-AZ | Single AZ |

---

## 6️⃣ Is EFS encrypted?

Yes:

* Encryption at rest (KMS)
* Encryption in transit (TLS)

---

## 7️⃣ Can EFS be mounted on Windows?

No, EFS supports **Linux only** (NFS-based).

---

## 8️⃣ What is a Mount Target?

A mount target allows EC2 instances in a subnet to connect to EFS.

---

## 9️⃣ Does EFS support auto-scaling?

Yes, storage automatically grows and shrinks.

---

## 🔟 Common Use Cases of EFS?

* Shared web content
* CMS (WordPress, Joomla)
* Container storage
* Big data processing

---

## 🏁 Final Summary

✔ Fully managed shared file system
✔ Scales automatically
✔ Multi-AZ & highly available
✔ Secure and reliable
✔ Ideal for distributed applications

---



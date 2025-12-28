# 🌐 Amazon Route 53 

Amazon Route 53 is a **highly available, scalable, and fully managed DNS (Domain Name System) service** provided by AWS. It helps route end users to internet applications by translating domain names into IP addresses.

---

## ✅ What is Amazon Route 53?

Amazon Route 53 is used to:

* Translate domain names into IP addresses
* Route traffic to AWS resources or external servers
* Perform health checks
* Enable intelligent traffic routing

---

## ⭐ Key Features

### 🔹 1. DNS Service

* Converts domain names (example.com) → IP addresses
* Ensures users reach the correct server

### 🔹 2. Domain Registration

* Buy and manage domains directly in AWS
* Supports .com, .in, .org, .net, etc.

### 🔹 3. Health Checks

* Automatically monitors application health
* Routes traffic only to healthy endpoints

### 🔹 4. Traffic Routing Policies

| Routing Policy | Description                           |
| -------------- | ------------------------------------- |
| Simple         | One record → one resource             |
| Weighted       | Split traffic by percentage           |
| Latency-based  | Routes users to lowest-latency region |
| Failover       | Primary → Secondary                   |
| Geolocation    | Routes based on user location         |
| Multi-value    | Returns multiple healthy records      |

---

## 📌 Why the Name “Route 53”?

DNS operates on **port 53**, hence the name Route 53.

---

## 🌍 DNS Resolution – How It Works

### 🧠 What is DNS?

DNS converts domain names into IP addresses.

Example:

```
google.com → 142.250.78.14
```

---

### 🔄 DNS Resolution Flow

1️⃣ User enters `www.example.com`
2️⃣ Browser checks local cache
3️⃣ Query sent to ISP DNS Resolver
4️⃣ Resolver contacts Root Server
5️⃣ Root server points to TLD (.com)
6️⃣ TLD returns Authoritative Name Server
7️⃣ Authoritative server returns IP
8️⃣ Browser connects to web server

---

## 🧠 Summary

DNS = Phonebook of the Internet
TTL controls cache duration
Improves speed and reliability

---

# 🧪 LAB – Route 53 + ALB + EC2 (Full Setup)

## 🎯 Objective

Map a custom domain (from GoDaddy) to an AWS Application Load Balancer using Route 53 and secure it using HTTPS.

---

## 🏗 Architecture Overview

```
User
 ↓
Route 53 (DNS)
 ↓
Application Load Balancer
 ↓
EC2 Instance
 ↓
Website
```

---

## 📋 Prerequisites

✔ EC2 instance running
✔ Website hosted
✔ Application Load Balancer created
✔ Domain purchased (GoDaddy)

---

## 🧪 STEP 1: Create Public Hosted Zone

1. Go to **Route 53 → Hosted Zones**
2. Click **Create Hosted Zone**
3. Enter your domain name
4. Type: **Public Hosted Zone**
5. Click **Create**

Route 53 creates:

* SOA record
* NS records

---

## 🧪 STEP 2: Update GoDaddy Nameservers

1. Login to GoDaddy
2. Go to **My Products → DNS**
3. Change Nameservers → Custom
4. Add Route 53 NS records:

Example:

```
ns-123.awsdns-45.com
ns-456.awsdns-78.org
ns-789.awsdns-12.net
ns-321.awsdns-90.co.uk
```

⏳ DNS propagation may take 5–60 minutes.

---

## 🧪 STEP 3: Get ALB DNS Name

1. Go to **EC2 → Load Balancers**
2. Select ALB
3. Copy DNS name:

```
my-alb-123456.ap-south-1.elb.amazonaws.com
```

---

## 🧪 STEP 4: Create Route 53 Records

### ✅ A Record (Root Domain)

* Type: A
* Alias: Yes
* Target: Application Load Balancer

### ✅ CNAME Record (www)

* Name: www
* Value: yourdomain.com

---

## 🧪 STEP 5: Request SSL Certificate (ACM)

1. Go to **AWS Certificate Manager**
2. Request public certificate
3. Add domains:

   * yourdomain.com
   * [www.yourdomain.com](http://www.yourdomain.com)
4. Choose DNS validation
5. Create records automatically in Route 53

Status changes from **Pending → Issued**

---

## 🧪 STEP 6: Attach SSL to ALB

1. EC2 → Load Balancers
2. Select ALB
3. Add Listener:

   * Protocol: HTTPS
   * Port: 443
   * Certificate: ACM
4. Forward to Target Group

---

## 🧪 STEP 7: Redirect HTTP → HTTPS

1. Edit HTTP (port 80) listener
2. Add redirect:

   * HTTPS
   * Port 443

---

## 🧪 STEP 8: Test Website

Open in browser:

```
https://yourdomain.com
https://www.yourdomain.com
```

✅ HTTPS works
✅ SSL lock visible

---

## 🧼 Cleanup (Optional)

* Delete Hosted Zone
* Remove ACM certificate
* Delete ALB
* Terminate EC2

---

# 🧠 Important Route 53 Concepts

| Feature      | Description              |
| ------------ | ------------------------ |
| Hosted Zone  | DNS records container    |
| Record Set   | DNS entry                |
| TTL          | Cache duration           |
| Alias Record | AWS-specific DNS mapping |
| Health Check | Monitors endpoint health |

---

# ❓ Interview Questions (Route 53)

### 1️⃣ What is Route 53?

DNS and domain management service.

### 2️⃣ Why use Route 53?

High availability, low latency, global DNS.

### 3️⃣ What routing policies exist?

Simple, Weighted, Latency, Failover, Geolocation.

### 4️⃣ What is an Alias record?

AWS-specific record pointing to AWS resources.

### 5️⃣ Difference between A and CNAME?

* A → IP address
* CNAME → another domain

### 6️⃣ What is TTL?

Time DNS record is cached.

### 7️⃣ What happens during failover?

Traffic moves to healthy resource automatically.

---

# ✅ Final Summary

✔ DNS management
✔ Domain routing
✔ Traffic control
✔ High availability
✔ Secure HTTPS setup
✔ Production-ready architecture

---



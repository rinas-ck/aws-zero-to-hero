# 🛡️ AWS WAF – Web Application Firewall 

Amazon Web Application Firewall (AWS WAF) helps protect your web applications from common web exploits and attacks that could affect availability, compromise security, or consume excessive resources.

Think of AWS WAF as a **security guard** that inspects every web request before it reaches your application.

---

## 📘 What is AWS WAF?

AWS WAF (Web Application Firewall) is a security service that allows you to **monitor, filter, and block HTTP/HTTPS requests** based on defined rules.

It protects:

* Websites
* APIs
* Web applications

From attacks such as:

* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Bot attacks
* DDoS (Layer 7)
* Malicious IP traffic

---

## 🎯 Why Use AWS WAF?

AWS WAF helps you:

✅ Protect applications from common attacks
✅ Control who can access your application
✅ Reduce downtime caused by malicious traffic
✅ Improve overall application security
✅ Easily integrate with AWS services

---

## 🏗️ Where Can AWS WAF Be Used?

AWS WAF can be attached to:

| Service                      | Description                |
| ---------------------------- | -------------------------- |
| 🌐 CloudFront                | Protect global CDN traffic |
| ⚖️ Application Load Balancer | Secure web apps            |
| 🔌 API Gateway               | Protect APIs               |
| 📡 AppSync                   | Protect GraphQL APIs       |

---

## 🧱 Core Components of AWS WAF

### 1️⃣ Web ACL (Access Control List)

The main firewall configuration.

Defines:

* Which requests are **Allowed**
* Which are **Blocked**
* Which are **Counted**

---

### 2️⃣ Rules

Rules define how traffic is evaluated.

Examples:

* Block specific IPs
* Allow office IPs
* Detect SQL injection
* Block XSS attacks

---

### 3️⃣ Rule Groups

A group of rules bundled together.

Types:

* **AWS Managed Rule Groups** (recommended)
* **Custom Rule Groups**

---

### 4️⃣ Managed Rules

Pre-built rules maintained by AWS:

* SQL Injection protection
* XSS protection
* Known bad inputs
* Bot detection

---

## 🔍 How AWS WAF Works (Flow)

1. User sends a request
2. Request reaches AWS WAF
3. WAF evaluates rules
4. Action is applied:

   * ✅ Allow
   * ❌ Block
   * 📊 Count
5. Request forwarded to application if allowed

---

## 📊 Monitoring AWS WAF

You can monitor WAF using:

* CloudWatch Metrics
* AWS WAF Logs
* Amazon Kinesis Data Firehose
* CloudWatch Dashboards

You can track:

* Allowed requests
* Blocked requests
* Rule match counts
* Traffic patterns

---

## 💰 AWS WAF Pricing (Simple)

You pay for:

* Number of Web ACLs
* Number of rules
* Number of requests inspected

💡 No upfront cost — pay as you go.

---

## 🧪 LAB: AWS WAF with Application Load Balancer (ALB)

### 🎯 Objective

Secure an Application Load Balancer using AWS WAF with:

* Allow rule
* Block rule
* CAPTCHA rule

---

## 🟦 Step 1 — Open AWS WAF Console

Go to:

```
AWS Console → WAF & Shield
```

---

## 🟩 Step 2 — Create IP Set (Allow Your IP)

1. Click **IP sets**
2. Click **Create IP set**
3. Name: `My-IP-Set`
4. Region: Same as ALB
5. Add your IP:

```
<your-public-ip>/32
```

6. Create IP set

---

## 🟧 Step 3 — Create Web ACL

1. Go to **Web ACLs**
2. Click **Create web ACL**
3. Name: `My-Web-ACL`
4. Region: Same as ALB
5. Resource type: **Application Load Balancer**
6. Select your ALB

---

## 🟥 Step 4 — Add Rules

### ✅ Rule 1: Allow Your IP

* Rule type: IP match
* IP set: `My-IP-Set`
* Action: **Allow**

---

### ❌ Rule 2: Block Test IP Range

* Rule type: IP match
* IP range example:

```
1.1.1.0/24
```

* Action: **Block**

---

### 🧩 Rule 3: CAPTCHA for High Traffic

* Rule type: Rate-based
* Limit: `100 requests / 5 minutes`
* Action: **CAPTCHA**

---

## 🟪 Step 5 — Rule Priority (Very Important)

Order:

1. Allow-My-IP
2. Block-Test-IP
3. Captcha-Unknown-IPs

Default action: **Allow**

---

## 🟦 Step 6 — Review & Create

Click **Create Web ACL**

WAF is now attached to your ALB.

---

## 🧪 Step 7 — Test WAF Rules

### ✅ Test Allow

Access your ALB DNS:

```
http://<ALB-DNS>
```

Page should load normally.

---

### ❌ Test Block

Try accessing using:

* VPN
* Proxy
* Different IP

You should see:

```
403 Forbidden
```

---

### 🔐 Test CAPTCHA

Send many requests quickly or refresh repeatedly.

A CAPTCHA challenge should appear.

---

## 🎉 Lab Completed Successfully!

You have implemented:

* IP-based filtering
* Rate limiting
* CAPTCHA protection
* Web ACL on ALB

---

# 🧠 AWS WAF Interview Questions & Answers

### 1️⃣ What is AWS WAF?

AWS WAF is a web application firewall that protects applications from web attacks.

---

### 2️⃣ What types of attacks does AWS WAF protect against?

SQL Injection, XSS, bots, brute force, and Layer 7 DDoS.

---

### 3️⃣ Where can AWS WAF be attached?

CloudFront, ALB, API Gateway, AppSync.

---

### 4️⃣ What is a Web ACL?

A set of rules that define allow/block behavior.

---

### 5️⃣ What is a Rule Group?

A collection of rules managed together.

---

### 6️⃣ What is a Managed Rule?

Predefined security rules created by AWS.

---

### 7️⃣ What is Rate-Based Rule?

Limits number of requests from an IP in a time window.

---

### 8️⃣ What is CAPTCHA in WAF?

Used to verify if traffic is human or bot.

---

### 9️⃣ Does AWS WAF protect against DDoS?

Yes, Layer 7 DDoS (works with AWS Shield).

---

### 🔟 Difference between AWS Shield and AWS WAF?

* **Shield** → DDoS protection
* **WAF** → Application-level filtering

---

## 🏁 Final Summary

✔ AWS WAF protects web applications
✔ Blocks malicious traffic
✔ Works with CloudFront, ALB, API Gateway
✔ Supports custom & managed rules
✔ Easy to scale and monitor

---



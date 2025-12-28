# 🌐 AWS CloudFront – 

AWS CloudFront is a **Content Delivery Network (CDN)** that securely delivers data, videos, applications, and APIs to users globally with low latency and high transfer speeds.

---

## 🚀 What is AWS CloudFront?

Amazon CloudFront is a globally distributed CDN service that caches and delivers content from **edge locations** close to users.

It works seamlessly with:

* Amazon S3
* EC2
* Application Load Balancer (ALB)
* API Gateway

---

## ⭐ Why Use CloudFront?

✔ Faster content delivery
✔ Reduced latency
✔ Global edge network
✔ Highly secure (HTTPS, WAF, Shield)
✔ Cost efficient
✔ Auto scaling

---

## 🗺️ CloudFront Architecture

User
↓
Nearest Edge Location
↓ (Cache Hit / Miss)
Origin (S3 / EC2 / ALB)
↓
Response delivered

---

## 🧩 Core Components

### 📦 Distribution

Main CloudFront configuration that defines how content is delivered.

### 🏗️ Origin

Backend source:

* S3
* EC2
* ALB
* Custom origin

### 🌍 Edge Locations

AWS global locations that cache content closer to users.

### ⚙️ Cache Behavior

Controls:

* HTTP methods
* TTL (Time To Live)
* HTTPS behavior

---

## 🔐 Security Features

| Feature                     | Description                    |
| --------------------------- | ------------------------------ |
| HTTPS                       | Secure encrypted communication |
| AWS WAF                     | Protects from attacks          |
| AWS Shield                  | DDoS protection                |
| Signed URLs                 | Restricts access               |
| OAC (Origin Access Control) | Secure private S3 access       |

---

## 🎯 Common Use Cases

| Use Case             | Example              |
| -------------------- | -------------------- |
| Website Acceleration | Faster page loads    |
| Video Streaming      | OTT platforms        |
| Image Delivery       | E-commerce           |
| API Acceleration     | Faster APIs          |
| Secure Content       | Paid/private content |

---

# 🧪 LAB 1 – CloudFront with EC2 (via ALB)

## 🎯 Objective

Serve content using **CloudFront → ALB → EC2**

---

## 🏁 Step 1: Launch EC2 Instance

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "<h1>Welcome to CloudFront</h1>" > /var/www/html/index.html
```

---

## 🏗️ Step 2: Create Application Load Balancer

1. Go to **EC2 → Load Balancers**
2. Click **Create Load Balancer**
3. Select **Application Load Balancer**
4. Scheme → Internet-facing
5. Select **2 Availability Zones**
6. Create target group and register EC2

---

## 🌍 Step 3: Create CloudFront Distribution

1. Open **CloudFront**
2. Click **Create Distribution**
3. Origin → Select ALB
4. Viewer protocol → Redirect HTTP to HTTPS
5. Create distribution

---

## ✅ Step 4: Test CloudFront

Open:

```
https://xxxxxxxx.cloudfront.net
```

---

# 🌐 LAB 2 – CloudFront + S3 (Using OAC)

---

## 📁 Folder Structure

```
index.html
assets/
  ├── css/
  ├── js/
images/
```

---

## 🧪 Step 1: Create S3 Bucket

* Create bucket
* Keep **Block Public Access ON**
* Upload website files

---

## 🧪 Step 2: Enable Static Website Hosting

* Properties → Static Website Hosting
* Index document: `index.html`

---

## 🧪 Step 3: Create CloudFront Distribution

* Origin → Select S3 bucket
* Enable **Origin Access Control (OAC)**

---

## 🧪 Step 4: Apply Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontAccess",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::ACCOUNT_ID:distribution/DISTRIBUTION_ID"
        }
      }
    }
  ]
}
```

---

## 🧪 Step 5: Invalidate Cache

```
CloudFront → Invalidations → /*
```

---

## 🧪 Step 6: Test Website

```
https://your-cloudfront-domain.cloudfront.net
```

✔ HTML loads
✔ CSS loads
✔ Images load

---

## 🚨 Common Issues & Fixes

| Issue           | Fix                    |
| --------------- | ---------------------- |
| CSS not loading | Fix bucket policy      |
| 403 Error       | Enable OAC             |
| Old content     | Invalidate cache       |
| HTTPS issue     | Attach ACM certificate |

---

# 📘 CloudFront Interview Questions & Answers

## 1️⃣ What is AWS CloudFront?

A global CDN that delivers content using edge locations with low latency.

---

## 2️⃣ What is an Edge Location?

A data center that caches and serves content close to users.

---

## 3️⃣ Difference between CloudFront and S3?

* S3 → Storage
* CloudFront → Content delivery

---

## 4️⃣ What is Origin?

The backend source where CloudFront fetches content (S3, ALB, EC2).

---

## 5️⃣ What is OAC?

Origin Access Control securely allows CloudFront to access private S3 buckets.

---

## 6️⃣ What is TTL?

Time content stays cached at edge locations.

---

## 7️⃣ What happens on cache miss?

CloudFront fetches data from origin and caches it.

---

## 8️⃣ What is Signed URL?

Used to restrict content access to authorized users.

---

## 9️⃣ CloudFront vs ALB?

* CloudFront = CDN
* ALB = Load balancer

---

## 🔟 Can CloudFront cache APIs?

Yes, it can cache API Gateway or ALB responses.

---

## 1️⃣1️⃣ How is security handled?

Using HTTPS, WAF, Shield, Signed URLs, OAC.

---

## 1️⃣2️⃣ How to reduce latency?

Use edge caching and reduce TTLs.

---

## 1️⃣3️⃣ What is Origin Shield?

Extra caching layer to reduce load on origin.

---

## 1️⃣4️⃣ What happens if origin is down?

Cached content may still be served.

---

## 1️⃣5️⃣ When to use CloudFront?

For global users, performance, security, and scalability.

---

# 🏁 FINAL SUMMARY

✔ Global CDN
✔ Fast & secure
✔ Integrates with S3, EC2, ALB
✔ Reduces latency & cost
✔ Ideal for production workloads

---




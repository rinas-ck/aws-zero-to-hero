# 📢 Amazon SNS (Simple Notification Service) 

Amazon Simple Notification Service (SNS) is a **fully managed messaging service** that enables applications to send messages and notifications to multiple subscribers instantly.

---

## 🔍 What is Amazon SNS?

Amazon SNS helps you:

* Send real-time alerts
* Build event-driven architectures
* Fan-out messages to multiple systems
* Trigger automated workflows
* Notify users via Email, SMS, or Mobile Push

---

## 🚀 Why Use Amazon SNS?

SNS is used to:

* Deliver system alerts
* Trigger serverless workflows
* Integrate AWS services
* Build decoupled architectures
* Notify users instantly

---

## 🔑 Core Concepts of SNS

### 🧵 1. Topic

A **Topic** is a communication channel where publishers send messages and subscribers receive them.

### Types of Topics

#### ✅ Standard Topic

* High throughput
* Best-effort ordering
* Possible duplicate messages
* Commonly used

#### ✅ FIFO Topic

* Guaranteed ordering
* No duplicates
* Lower throughput
* Used when message order matters

---

### 📨 2. Publisher

A service or application that sends messages to a topic.

Examples:

* EC2
* CloudWatch
* Lambda
* Custom applications

---

### 👤 3. Subscriber

Receives messages from SNS.

Supported subscribers:

* Email
* SMS
* HTTP / HTTPS
* SQS
* Lambda

---

### 💬 4. Message

The actual data sent to subscribers.

---

## 🔄 SNS Message Flow

```
Publisher → SNS Topic → Subscribers
```

Steps:

1. Create a Topic
2. Subscribe endpoints
3. Publish messages
4. SNS delivers messages

---

## 💡 Common SNS Use Cases

| Use Case          | Example                  |
| ----------------- | ------------------------ |
| Alerts            | CloudWatch → SNS → Email |
| Fan-out           | SNS → SQS → Workers      |
| Serverless        | SNS → Lambda             |
| Notifications     | Email / SMS              |
| Event-driven apps | Microservices            |

---

# 🧪 LAB 1 – SNS + S3 Event Notification

### 🎯 Goal

Send an email notification whenever an object is uploaded or deleted in an S3 bucket.

---

## ✅ Step 1: Create SNS Topic

1. Go to **SNS → Topics**
2. Click **Create Topic**
3. Type: **Standard**
4. Name: `s3-event-alerts`
5. Click **Create Topic**

---

## ✅ Step 2: Create Subscription

1. Open the topic
2. Click **Create Subscription**
3. Protocol: **Email**
4. Endpoint: *your email*
5. Confirm the subscription via email

---

## ✅ Step 3: Create S3 Bucket

1. Go to **S3 → Create Bucket**
2. Provide a unique name
3. Create the bucket

---

## ✅ Step 4: Configure Event Notification

1. Open bucket → **Properties**
2. Go to **Event Notifications**
3. Click **Create event notification**
4. Choose events:

   * ObjectCreated
   * ObjectRemoved
5. Destination → **SNS Topic**
6. Save

---

## ✅ Step 5: Test the Setup

Upload or delete a file in the bucket.

📩 You will receive an email notification instantly.

---

# 🧪 LAB 2 – SNS + CloudWatch Alarm (CPU Alert)

### 🎯 Goal

Send an email when EC2 CPU usage exceeds 50%.

---

## ✅ Step 1: Create SNS Topic

* Name: `EC2-CPU-Alert`
* Type: Standard

---

## ✅ Step 2: Create Subscription

* Protocol: Email
* Confirm subscription

---

## ✅ Step 3: Launch EC2 Instance

* Amazon Linux
* Enable **Detailed Monitoring**

---

## ✅ Step 4: Create CloudWatch Alarm

1. Go to **CloudWatch → Alarms → Create Alarm**
2. Select **CPUUtilization**
3. Threshold: **Greater than 50%**
4. Period: **1 minute**
5. Action: Send notification to SNS topic

---

## ✅ Step 5: Test Alarm

Run on EC2:

```bash
sudo yum install stress -y
stress --cpu 2
```

📩 Email will be triggered when CPU exceeds 50%.

---

# 📊 CloudWatch + SNS Integration

CloudWatch can trigger SNS for:

* Alarms
* Events
* Auto Scaling actions

Example flow:

```
CloudWatch → SNS → Email / Lambda / SQS
```

---

# 🧠 Important SNS Concepts

| Concept    | Description       |
| ---------- | ----------------- |
| Topic      | Messaging channel |
| Publisher  | Sends messages    |
| Subscriber | Receives messages |
| Message    | Actual payload    |
| Protocol   | Delivery method   |

---

# 📚 SNS Interview Questions

### 1️⃣ What is Amazon SNS?

A managed pub/sub messaging service.

### 2️⃣ Difference between SNS and SQS?

* SNS = push-based
* SQS = pull-based

### 3️⃣ What is fan-out?

One message delivered to multiple subscribers.

### 4️⃣ Can SNS trigger Lambda?

Yes.

### 5️⃣ Does SNS support SMS?

Yes.

### 6️⃣ What is FIFO SNS?

Ordered, exactly-once delivery.

### 7️⃣ What protocols does SNS support?

Email, SMS, HTTP, HTTPS, SQS, Lambda.

### 8️⃣ What is SNS used for?

Alerts, automation, notifications.

---

# ✅ Final Summary

✔ Event-driven messaging
✔ Email & SMS notifications
✔ Integration with S3, EC2, CloudWatch
✔ Real-time alerting
✔ Highly scalable and reliable

---




# 📘 Amazon SQS (Simple Queue Service) 

Amazon **Simple Queue Service (SQS)** is a fully managed message queuing service that enables **asynchronous communication** between distributed application components.

It helps decouple microservices, improve reliability, and handle high traffic without data loss.

---

## 🧭 What is Amazon SQS?

Amazon SQS allows applications to send, store, and receive messages between software components without losing messages or requiring components to be always available.

➡️ SQS acts as a **temporary message buffer** between producers and consumers.

---

## 🚀 Why Use SQS?

✔ Decouples microservices
✔ Prevents system overload
✔ Guarantees message durability
✔ Scales automatically
✔ Fault tolerant & highly available

---

## 🟦 Types of SQS Queues

### 🔹 1. Standard Queue (Default)

* Unlimited throughput
* At-least-once delivery
* Best-effort ordering
* Suitable for high-scale systems

### 🔹 2. FIFO Queue (First-In-First-Out)

* Guaranteed ordering
* Exactly-once processing
* Lower throughput
* Supports **Message Group ID**

✔ Use FIFO when **order matters** (payments, transactions).

---

## 🚀 How SQS Works (Simple Flow)

1️⃣ Producer sends a message
2️⃣ SQS stores the message securely
3️⃣ Consumer polls the queue
4️⃣ Consumer processes the message
5️⃣ Message is deleted

➡️ If not deleted, the message becomes visible again after **Visibility Timeout**.

---

## 🔐 Important Concepts

### 📨 Message

* Maximum size: **256 KB**
* Can be JSON, XML, or plain text

### ⏲️ Visibility Timeout

* Period during which a message is hidden after being read
* Prevents duplicate processing

### 📅 Message Retention

* How long SQS stores messages
* Range: **1 minute to 14 days**

### 🗳️ Dead Letter Queue (DLQ)

* Stores messages that failed processing after multiple retries
* Helps debugging and error handling

### 🔁 Long Polling

* Waits for messages before responding
* Reduces empty responses
* Saves cost

---

## 🛠️ Features of Amazon SQS

✔ Fully managed
✔ Highly scalable
✔ Secure (IAM, encryption, VPC endpoint)
✔ Durable (stored across multiple AZs)
✔ Cost-efficient

---

## 🎯 When to Use SQS?

* Microservices communication
* Order processing systems
* Image/video processing
* Logging and monitoring
* Background job execution
* Event-driven architectures

---

## 🧰 SQS Basic CLI Commands

```bash
# Create a queue
aws sqs create-queue --queue-name MyQueue

# List queues
aws sqs list-queues

# Send message
aws sqs send-message --queue-url <QUEUE_URL> --message-body "Hello"

# Receive message
aws sqs receive-message --queue-url <QUEUE_URL>

# Delete message
aws sqs delete-message --queue-url <QUEUE_URL> --receipt-handle <HANDLE>
```

---

# 🧪 LAB – Amazon SQS Hands-On

This lab demonstrates how to create, send, receive, and delete messages in SQS.

---

## 🟩 Step 1 — Open SQS

1. Login to AWS Console
2. Search for **SQS**
3. Open **Simple Queue Service**

---

## 🟩 Step 2 — Create a Queue

1. Click **Create Queue**
2. Choose **Standard Queue**
3. Queue name:

```
my-demo-queue
```

4. Click **Create Queue**

✔ Queue created successfully.

---

## 🟩 Step 3 — Send a Message

1. Open the queue
2. Click **Send and receive messages**
3. Enter message body:

```
Hello from SQS
```

4. Click **Send message**

---

## 🟩 Step 4 — Receive the Message

1. Click **Poll for messages**
2. Message appears
3. Click the message to view content

✔ Message received successfully.

---

## 🟩 Step 5 — Delete the Message

1. Select the message
2. Click **Delete**
3. Confirm deletion

✔ Message removed from queue.

---

## 🟩 Step 6 — Clean Up

1. Go back to SQS console
2. Select the queue
3. Click **Delete queue**

✔ Cleanup completed.

---

# 🧠 SQS Interview Questions & Answers

---

### 1️⃣ What is Amazon SQS?

A fully managed message queuing service used to decouple and scale microservices.

---

### 2️⃣ Difference between Standard and FIFO queues?

| Feature | Standard | FIFO |
|------|------|
| Ordering | Best-effort | Guaranteed |
| Delivery | At-least-once | Exactly-once |
| Throughput | High | Lower |

---

### 3️⃣ What is Visibility Timeout?

Time during which a message is hidden after being read.

---

### 4️⃣ What happens if a message is not deleted?

It becomes visible again after the visibility timeout.

---

### 5️⃣ What is a Dead Letter Queue?

A queue that stores messages that fail processing multiple times.

---

### 6️⃣ What is Long Polling?

Waiting for messages instead of repeatedly polling.

---

### 7️⃣ How is SQS secured?

* IAM policies
* Encryption (KMS)
* VPC Endpoints

---

### 8️⃣ Can SQS guarantee message order?

Only FIFO queues guarantee ordering.

---

### 9️⃣ What is the maximum message size?

256 KB.

---

### 🔟 When should you use SQS?

* Decoupled systems
* Async processing
* Background jobs
* Fault-tolerant workflows

---

## 🏁 Final Summary

✔ Fully managed message queue
✔ Supports Standard & FIFO queues
✔ Durable, secure & scalable
✔ Ideal for decoupling microservices
✔ Easy integration with AWS services

---



# ⚡ AWS Lambda 

AWS Lambda is a **serverless compute service** that lets you run code without provisioning or managing servers. You only pay for the compute time you consume.

---

## 📘 What is AWS Lambda?

AWS Lambda runs your code **in response to events** and automatically manages compute resources.

You simply:

* Upload code
* Configure a trigger
* AWS runs it automatically

---

## 🚀 Why Use AWS Lambda?

✅ No server management
✅ Automatic scaling
✅ Pay only for execution time
✅ Integrates with AWS services
✅ Highly available & fault-tolerant

---

## 🧠 How Lambda Works

1. Upload your function code
2. Configure a trigger (S3, API Gateway, etc.)
3. Event occurs
4. Lambda executes your function
5. Logs stored in CloudWatch

---

## 🧩 Core Concepts

### 🔹 Function

Your code logic.

### 🔹 Trigger (Event Source)

What invokes Lambda:

* S3 upload
* API Gateway
* DynamoDB stream
* CloudWatch schedule

### 🔹 Execution Role (IAM)

Allows Lambda to access AWS services.

### 🔹 Memory & Timeout

* Memory: 128 MB – 10 GB
* Timeout: up to 15 minutes

### 🔹 Cold vs Warm Start

* Cold start → first execution (slower)
* Warm start → already running (fast)

---

## 🛠 Supported Runtimes

* Python
* Node.js
* Java
* Go
* .NET
* Ruby

---

## 💰 Pricing

You pay for:

* Number of requests
* Execution duration

🎁 First 1 million requests per month are FREE.

---

## 🎯 Common Use Cases

* Backend APIs
* File processing
* Automation jobs
* Image resizing
* Notifications
* Serverless websites

---

# 🧪 LAB 1 — S3 Trigger → Lambda → CloudWatch

## 🎯 Goal

Trigger a Lambda function whenever a file is uploaded to S3.

---

## ✅ Step 1: Create S3 Bucket

* Go to **S3 Console**
* Create bucket

  ```
  lambda-test-buck-909090
  ```

---

## ✅ Step 2: Create Lambda Function

* Go to **Lambda Console**
* Click **Create function**
* Name: `s3-lambda-logger`
* Runtime: **Python 3.12**
* Click **Create**

---

## ✅ Step 3: Add Lambda Code

```python
import json
import urllib.parse
import boto3

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = urllib.parse.unquote_plus(event['Records'][0]['s3']['object']['key'])
    print(f"File uploaded: {bucket}/{key}")
    return {"status": "success"}
```

Click **Deploy**

---

## ✅ Step 4: Add Permissions (IAM Role)

Attach these policies:

* `AWSLambdaBasicExecutionRole`
* Custom policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::lambda-test-buck-909090",
        "arn:aws:s3:::lambda-test-buck-909090/*"
      ]
    }
  ]
}
```

---

## ✅ Step 5: Add S3 Trigger

* Go to Lambda → Triggers
* Add trigger → S3
* Bucket: `lambda-test-buck-909090`
* Event type: **All object create events**

---

## ✅ Step 6: Test

Upload any file to S3
Go to **CloudWatch Logs**
You will see event logs 🎉

---

# 🧪 LAB 2 – Image Resize Using Lambda (Advanced)

## 🎯 Goal

Upload image → Lambda resizes → stores in another bucket.

---

## 🧱 Architecture

```
S3 (Source) → Lambda → S3 (Destination)
```

---

## ✅ Step 1: Create Buckets

* Source: `my-lambda-source-bucket`
* Destination: `my-lambda-dest-bucket`

---

## ✅ Step 2: Create IAM Role

Attach:

* CloudWatch Logs access
* S3 read/write permissions

---

## ✅ Step 3: Lambda Code (Python + Pillow)

```python
import boto3
from PIL import Image
import io

s3 = boto3.client('s3')

def lambda_handler(event, context):
    src_bucket = event['Records'][0]['s3']['bucket']['name']
    src_key = event['Records'][0]['s3']['object']['key']
    dst_bucket = src_bucket + "-resized"

    obj = s3.get_object(Bucket=src_bucket, Key=src_key)
    img = Image.open(obj['Body'])
    img.thumbnail((200, 200))

    buffer = io.BytesIO()
    img.save(buffer, 'JPEG')
    buffer.seek(0)

    s3.put_object(
        Bucket=dst_bucket,
        Key="thumb-" + src_key,
        Body=buffer,
        ContentType="image/jpeg"
    )

    return {"status": "success"}
```

---

## ✅ Step 4: Configure Trigger

* Source bucket → Event notification
* Event: PUT
* Destination: Lambda

---

## ✅ Step 5: Test

Upload an image → thumbnail appears in destination bucket.

---

# 🧠 Lambda Interview Q&A

### 1️⃣ What is AWS Lambda?

Serverless compute service that runs code without managing servers.

### 2️⃣ What triggers Lambda?

S3, API Gateway, EventBridge, DynamoDB, SQS, SNS.

### 3️⃣ What is cold start?

Initial startup delay when function runs after inactivity.

### 4️⃣ Max execution time?

15 minutes.

### 5️⃣ What is Lambda concurrency?

Number of parallel executions.

### 6️⃣ How does Lambda scale?

Automatically based on incoming requests.

### 7️⃣ What is IAM role in Lambda?

Defines permissions Lambda has.

### 8️⃣ Can Lambda access VPC?

Yes, via VPC configuration.

### 9️⃣ What happens if Lambda fails?

Retry or send to DLQ depending on trigger.

### 🔟 Where are logs stored?

Amazon CloudWatch Logs.

---

## 🧾 Final Summary

✔ Serverless computing
✔ Event-driven execution
✔ Auto scaling & high availability
✔ Pay per execution
✔ Easy integration with AWS services

---


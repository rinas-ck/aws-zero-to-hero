# 🌐 AWS CORS (Cross-Origin Resource Sharing) 

CORS (Cross-Origin Resource Sharing) is a **browser security mechanism** that controls which websites can access resources hosted on another domain.

It is commonly used with **S3, API Gateway, CloudFront, and backend APIs**.

---

## 📘 What is CORS?

CORS allows a web application running on one origin (domain) to access resources from another origin **only if explicitly permitted**.

👉 Without proper CORS headers, browsers will **block requests**.

---

## 🧠 Why CORS Exists?

Browsers enforce **Same-Origin Policy** to protect users from malicious websites.

Without CORS:

* Any website could read private data
* Security vulnerabilities would exist

CORS tells the browser:

> “This domain is allowed to access this resource.”

---

## 🟦 When Does CORS Trigger?

CORS is triggered when:

* `frontend.com → api.backend.com`
* `localhost:3000 → AWS API Gateway`
* `CloudFront → S3`
* Any **cross-domain request**

---

## 📌 Important CORS Concepts

### 🔹 1. Origin

Origin = **Protocol + Domain + Port**

Example:

```
https://example.com:443
```

---

### 🔹 2. Access-Control-Allow-Origin

Defines which origin can access the resource.

Example:

```
Access-Control-Allow-Origin: https://myapp.com
```

Allow all:

```
Access-Control-Allow-Origin: *
```

---

### 🔹 3. Access-Control-Allow-Methods

Defines allowed HTTP methods:

```
GET, POST, PUT, DELETE, OPTIONS
```

---

### 🔹 4. Access-Control-Allow-Headers

Defines allowed request headers:

```
Content-Type, Authorization
```

---

### 🔹 5. Preflight Request (OPTIONS)

Before sending actual data, the browser sends an **OPTIONS** request.

If the server responds correctly → request proceeds.

---

## 🛠️ CORS in AWS Services

### ✅ 1. Amazon S3 (Static Content)

Used when hosting:

* Images
* CSS
* JavaScript
* Static websites

**Example CORS policy:**

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": []
  }
]
```

---

### ✅ 2. API Gateway

Enable CORS in API Gateway to allow frontend access.

Typical response headers:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET,POST,OPTIONS
Access-Control-Allow-Headers: Content-Type
```

---

### ✅ 3. CloudFront

CloudFront must forward headers:

* Origin
* Access-Control-Request-Headers
* Access-Control-Request-Method

Otherwise, CORS fails even if S3/API is configured correctly.

---

## 🎯 Simple Example

Frontend:

```
http://localhost:3000
```

Backend:

```
https://abc123.execute-api.ap-south-1.amazonaws.com
```

If backend does not return:

```
Access-Control-Allow-Origin: http://localhost:3000
```

👉 Browser blocks the request.

---

## ⚠️ Common CORS Errors

| Error                            | Meaning                 |
| -------------------------------- | ----------------------- |
| No `Access-Control-Allow-Origin` | CORS not enabled        |
| Blocked by CORS policy           | Browser blocked request |
| Preflight failed                 | OPTIONS not handled     |

---

# 🧪 LAB – Access S3 Object Using CORS

---

## 🎯 Goal

Load an image from S3 into a webpage using CORS.

---

## 🟩 Step 1 — Create S3 Bucket

1. Go to **S3 Console**
2. Click **Create bucket**
3. Name: `my-cors-demo-bucket-009`
4. Create bucket

---

## 🟩 Step 2 — Upload Image

Upload:

```
image.png
```

---

## 🟩 Step 3 — Disable Block Public Access

Go to:

```
Bucket → Permissions → Block public access
```

Disable all options.

---

## 🟩 Step 4 — Add Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-cors-demo-bucket-009/*"
    }
  ]
}
```

---

## 🟩 Step 5 — Add CORS Configuration

Go to:

```
Permissions → CORS configuration
```

Paste:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": []
  }
]
```

---

## 🟩 Step 6 — Test Direct Access

Open:

```
https://my-cors-demo-bucket-009.s3.amazonaws.com/image.png
```

---

## 🟩 Step 7 — Create HTML Test Page

```html
<!DOCTYPE html>
<html>
<head>
  <title>CORS Test</title>
</head>
<body>

<h2>Load Image from S3</h2>

<button onclick="loadImage()">Load Image</button>

<br><br>

<img id="img" width="300" style="display:none;">

<script>
function loadImage() {
  document.getElementById("img").src =
  "https://my-cors-demo-bucket-009.s3.amazonaws.com/image.png";
  document.getElementById("img").style.display = "block";
}
</script>

</body>
</html>
```

---

## 🎉 Success!

Your browser successfully loads the S3 image using **CORS**.

---

# 🧠 CORS Interview Questions & Answers

---

### 1️⃣ What is CORS?

A browser security mechanism that controls cross-origin requests.

---

### 2️⃣ Why is CORS required?

To prevent unauthorized access between different websites.

---

### 3️⃣ What causes CORS errors?

Missing or incorrect response headers.

---

### 4️⃣ What is a preflight request?

An OPTIONS request sent before actual request.

---

### 5️⃣ What is Access-Control-Allow-Origin?

Defines which domain can access the resource.

---

### 6️⃣ Does CORS affect backend-to-backend calls?

No. CORS is enforced by browsers only.

---

### 7️⃣ Can CORS be disabled?

Not recommended. It protects users from attacks.

---

### 8️⃣ What AWS services commonly use CORS?

S3, API Gateway, CloudFront.

---

### 9️⃣ What happens if CORS is misconfigured?

Browser blocks request even if backend works.

---

### 🔟 Best practice for CORS?

Allow only trusted domains instead of "*".

---

## 🏁 Final Summary

✔ CORS is a browser security feature
✔ Used in S3, API Gateway, CloudFront
✔ Controls cross-origin access
✔ Requires correct headers
✔ Essential for frontend–backend communication

---



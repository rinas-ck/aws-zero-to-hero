# 📧 AWS SES (Simple Email Service) 

Amazon Simple Email Service (SES) is a **fully managed email service** that enables you to send transactional, notification, and marketing emails at scale with high deliverability and low cost.

---

## ⭐ What is AWS SES?

AWS SES is used to send:

* OTPs / Password reset emails
* Notifications
* Marketing & promotional emails
* Bulk emails

It removes the need to manage your own SMTP servers.

---

## ⭐ Why Use AWS SES?

✅ Very low cost
✅ High deliverability
✅ Highly scalable
✅ Secure & reliable
✅ Integrated with AWS services
✅ Supports SMTP, API & SDK

---

## ⭐ How AWS SES Works (Simple Flow)

```
Application → SES → Recipient Email Inbox
```

Steps:

1. Verify email or domain
2. Send email via SMTP / API
3. SES delivers email
4. Delivery feedback is logged

---

## ⭐ Ways to Send Emails

| Method  | Description                         |
| ------- | ----------------------------------- |
| SMTP    | Works like traditional email server |
| AWS SDK | Secure & recommended                |
| AWS CLI | Good for testing                    |
| Console | Simple testing                      |

---

## ⭐ Sandbox vs Production Mode

### 🧪 Sandbox Mode

* Can send emails **only to verified identities**
* Low sending limits
* Used for testing

### 🚀 Production Mode

* Send to **any email address**
* Higher sending limits
* Used for real applications

---

## ⭐ Security in SES

* Email & Domain Verification
* SPF, DKIM, DMARC support
* IAM-based permissions
* TLS encryption
* Bounce & complaint handling via SNS

---

## ⭐ Common Use Cases

* OTP & authentication emails
* Order confirmations
* Marketing campaigns
* System alerts
* Bulk notifications

---

# 🧪 LAB – AWS SES Step-by-Step (Beginner Friendly)

---

## 🎯 Objective

* Verify email identity
* Send test email
* Use SMTP credentials
* Monitor delivery

---

## 🧪 Step 1: Open SES Console

1. Login to AWS Console
2. Search **SES**
3. Select region (example: `us-east-1`)

---

## 🧪 Step 2: Verify Email Address

1. Go to **Verified Identities**
2. Click **Create Identity**
3. Choose **Email Address**
4. Enter your email
5. Click **Create Identity**

📩 Check your inbox and confirm verification.

---

## 🧪 Step 3: Create SMTP Credentials

1. Go to **SMTP Settings**
2. Click **Create SMTP Credentials**
3. Save:

   * SMTP Username
   * SMTP Password
   * Endpoint

Example:

```
email-smtp.us-east-1.amazonaws.com
```

---

## 🧪 Step 4: Send a Test Email (Console)

1. Go to **Email Sending → Send Test Email**
2. Choose:

   * From: Verified Email
   * To: Verified Email
3. Enter subject & message
4. Click **Send Email**

✅ Email will be delivered successfully.

---

## 🧪 Step 5: Send Email Using SMTP (Optional)

Use these settings in any email client:

| Field      | Value                              |
| ---------- | ---------------------------------- |
| Server     | email-smtp.us-east-1.amazonaws.com |
| Port       | 587                                |
| Encryption | TLS                                |
| Username   | SMTP Username                      |
| Password   | SMTP Password                      |

---

## 🧪 Step 6: View Sending Statistics

Go to:

**SES → Sending Statistics**

View:

* Emails sent
* Bounces
* Complaints

---

# 🧠 AWS SES Interview Questions & Answers

### 1️⃣ What is AWS SES?

AWS SES is a scalable cloud email service used to send transactional, marketing, and notification emails.

---

### 2️⃣ What is SES sandbox mode?

A restricted mode where emails can only be sent to verified identities.

---

### 3️⃣ How do you move SES to production?

Request production access from AWS SES console.

---

### 4️⃣ What is the difference between SES and SNS?

* SES → Email sending
* SNS → Notifications (SMS, email, push)

---

### 5️⃣ How does SES handle spam?

Using:

* DKIM
* SPF
* DMARC
* Reputation monitoring

---

### 6️⃣ What protocols does SES support?

SMTP, HTTPS (API), SDKs.

---

### 7️⃣ Can SES send bulk emails?

Yes, SES is designed for high-volume email sending.

---

### 8️⃣ What is bounce handling?

SES detects undelivered emails and tracks bounces automatically.

---

### 9️⃣ How do you secure SES?

IAM policies, verified identities, TLS encryption.

---

### 🔟 Is SES serverless?

Yes — no infrastructure management required.

---

## 🧾 Summary

✔ AWS SES is a powerful email service
✔ Supports SMTP, SDK, CLI
✔ Secure, scalable, and cost-effective
✔ Ideal for OTPs, alerts, marketing
✔ Easy integration with AWS services

---



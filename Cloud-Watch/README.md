# ☁️ Amazon CloudWatch – Zero to Hero

Amazon CloudWatch is a monitoring and observability service that provides visibility into AWS resources, applications, and services.

---

## 🔍 What is Amazon CloudWatch?

Amazon CloudWatch helps you:

* Monitor performance
* Detect issues
* Trigger automated actions
* Visualize metrics and logs

---

## ⭐ Core Features

* **Metrics** – Monitor CPU, memory, disk, and network usage
* **Logs** – Collect and analyze system & application logs
* **Alarms** – Trigger actions when thresholds are crossed
* **Dashboards** – Visualize metrics in real time
* **Events** – React to AWS service state changes

---

## 📊 CloudWatch Metrics

Metrics are time-series data points.

### Common EC2 Metrics

* CPUUtilization
* NetworkIn / NetworkOut
* DiskReadOps / DiskWriteOps
* StatusCheckFailed

---

## 📁 CloudWatch Logs

Used to collect logs from:

* EC2
* Lambda
* ECS
* Custom applications

### Components

* **Log Group** → Collection of logs
* **Log Stream** → Sequence of log events

---

## 🚨 CloudWatch Alarms

CloudWatch alarms monitor metrics and trigger actions.

### Alarm States

| State             | Description       |
| ----------------- | ----------------- |
| OK                | Normal            |
| ALARM             | Threshold crossed |
| INSUFFICIENT_DATA | Not enough data   |

### Alarm Actions

* Send SNS notifications
* Trigger Auto Scaling
* Invoke Lambda

---

# 🧪 LAB 1 – Monitor EC2 Using CloudWatch

### Step 1: Launch EC2

* Launch Amazon Linux
* Enable **Detailed Monitoring**

### Step 2: View Metrics

Go to:

```
CloudWatch → Metrics → EC2 → Per-Instance Metrics
```

Select:

* CPUUtilization

### Step 3: Generate Load

```bash
sudo yum install stress -y
stress --cpu 20 --timeout 300
```

✅ CPU usage will increase in CloudWatch.

---

# 🧪 LAB 2 – Create CloudWatch Alarm

### Steps:

1. Open **CloudWatch → Alarms**
2. Click **Create Alarm**
3. Select **CPUUtilization**
4. Set threshold to **80%**
5. Evaluation period → **5 minutes**
6. Create Alarm

---

# 📊 CloudWatch Dashboards

Dashboards allow visual monitoring of metrics.

You can display:

* CPU Utilization
* Network In / Out
* Disk I/O

---

# 🧪 LAB 3 – Create Dashboard

1. Go to **CloudWatch → Dashboards**
2. Click **Create Dashboard**
3. Add widgets
4. Save dashboard

---

# 🔁 CloudWatch + Auto Scaling

CloudWatch integrates with Auto Scaling Groups.

### Example:

* CPU > 70% → Scale Out
* CPU < 30% → Scale In

---

# 🧪 LAB 4 – Target Tracking Policy

1. Create Auto Scaling Group
2. Select **Target Tracking Policy**
3. Metric: `CPUUtilization`
4. Target value: `50%`

---

# 📈 Step Scaling Policy

| CPU Usage | Action            |
| --------- | ----------------- |
| > 60%     | Add 1 instance    |
| > 80%     | Add 2 instances   |
| < 40%     | Remove 1 instance |

---

# 📊 CloudWatch Logs Insights

Query logs using:

```sql
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

---

# 📊 CloudWatch + Grafana

### Steps:

1. Launch EC2 for Grafana
2. Install Grafana
3. Add CloudWatch as Data Source
4. Attach IAM Role:

   * `CloudWatchFullAccess`
5. Create dashboards

---

# 🔐 Required IAM Permissions

```
CloudWatchFullAccess
CloudWatchLogsFullAccess
```

---

# ❓ CloudWatch Interview Questions

### 1️⃣ What is CloudWatch?

Monitoring and observability service for AWS.

### 2️⃣ What metrics does EC2 send?

CPU, Network, Disk, Status checks.

### 3️⃣ What is a CloudWatch Alarm?

Triggers actions when a metric crosses a threshold.

### 4️⃣ What is a Log Group?

Collection of log streams.

### 5️⃣ What is a Log Stream?

Sequence of log events.

### 6️⃣ What is Detailed Monitoring?

1-minute metric granularity.

### 7️⃣ Can CloudWatch trigger Auto Scaling?

Yes.

### 8️⃣ Can CloudWatch invoke Lambda?

Yes.

### 9️⃣ What is a Dashboard?

Visual representation of metrics.

### 🔟 What is CloudWatch Logs Insights?

Query engine for analyzing logs.

---

# ✅ Summary

✔ Metrics
✔ Logs
✔ Alarms
✔ Dashboards
✔ Auto Scaling
✔ Interview Questions

---

# 🎉 END OF CLOUDWATCH README




# ☁️ Amazon CloudWatch 

Amazon CloudWatch is a **monitoring and observability service** that provides visibility into AWS resources, applications, and services.

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

✅ CPU usage increases in CloudWatch.

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

Dashboards provide real-time visualization.

You can display:

* CPU Utilization
* Network In / Out
* Disk I/O

---

# 🧪 LAB 3 – Create Dashboard

1. Go to **CloudWatch → Dashboards**
2. Click **Create Dashboard**
3. Add widgets (Line, Number, Graph)
4. Save dashboard

---

# 🔁 CloudWatch + Auto Scaling

CloudWatch integrates with Auto Scaling.

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

Analyze logs using queries:

```sql
fields @timestamp, @message
| sort @timestamp desc
| limit 20
```

---

# 📊 CloudWatch + Grafana (FULL GUIDE)

Grafana is an open-source visualization tool used with CloudWatch.

---

## 🔧 Why Use Grafana?

* Advanced dashboards
* Real-time monitoring
* Multi-data source support
* Better visualization than default CloudWatch

---

## 🧱 Architecture

```
EC2 → CloudWatch → Grafana → Dashboard
```

---

## 🧪 LAB 5 – Install Grafana on EC2

### Step 1: Launch EC2

* Amazon Linux 2
* Open port **3000** in Security Group

### Step 2: Install Grafana

```bash
sudo yum install -y grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

### Step 3: Access Grafana

```
http://<EC2-PUBLIC-IP>:3000
```

Default credentials:

```
Username: admin
Password: admin
```

---

## 🔑 Configure CloudWatch as Data Source

### Step 1:

Go to:

```
Configuration → Data Sources → Add data source
```

### Step 2:

Select **Amazon CloudWatch**

### Step 3:

Authentication:

* Use **IAM Role**
* Attach policy: `CloudWatchFullAccess`

---

## 🔐 Required IAM Permissions

Attach this role to EC2:

```
CloudWatchFullAccess
```

---

## 📊 Create Dashboard in Grafana

1. Go to **Dashboards → Create New**
2. Add Visualization
3. Select **CloudWatch**
4. Choose metrics:

   * CPUUtilization
   * NetworkIn / Out
   * DiskReadOps
5. Save dashboard

---

## 📈 Grafana Visualization Types

* Time series
* Gauge
* Bar chart
* Heatmap

---

## 🚨 Grafana Alerts

Grafana can send alerts via:

* Email
* Slack
* Webhooks

---

## ⭐ CloudWatch vs Grafana

| Feature | CloudWatch | Grafana |
|------|-----------|
| Native AWS | ✅ | ❌ |
| Custom Visualization | ❌ | ✅ |
| Advanced Dashboards | ❌ | ✅ |
| Alerting | Basic | Advanced |

---

# ❓ CloudWatch Interview Questions

### 1️⃣ What is CloudWatch?

Monitoring and observability service for AWS resources.

### 2️⃣ What metrics does EC2 send?

CPU, network, disk, and status checks.

### 3️⃣ What is CloudWatch Alarm?

Triggers action when threshold is crossed.

### 4️⃣ What is Log Group?

A collection of log streams.

### 5️⃣ What is Log Stream?

Sequence of log events.

### 6️⃣ What is Detailed Monitoring?

1-minute metric granularity.

### 7️⃣ Can CloudWatch trigger Auto Scaling?

Yes.

### 8️⃣ What is CloudWatch Logs Insights?

Query engine for logs.

### 9️⃣ What is CloudWatch Dashboard?

Visual representation of metrics.

### 🔟 Difference between Alarm & Event?

Alarm monitors metrics; Event reacts to state changes.

---

# ✅ Final Summary

✔ Metrics
✔ Logs
✔ Alarms
✔ Dashboards
✔ Auto Scaling
✔ Grafana Integration
✔ Interview Questions

---





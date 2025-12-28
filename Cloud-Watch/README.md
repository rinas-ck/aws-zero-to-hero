📊 Amazon CloudWatch – Zero to Hero (Complete Guide)
🔍 What is Amazon CloudWatch?

Amazon CloudWatch is a monitoring and observability service that provides real-time insights into AWS resources, applications, and services.

It helps you:

Monitor performance

Detect issues

Trigger automation

Analyze logs & metrics

🎯 Key Features of CloudWatch
Feature	Description
Metrics	Numeric performance data (CPU, Network, Disk, etc.)
Alarms	Triggers actions when thresholds are breached
Logs	Collects application and system logs
Dashboards	Visual representation of metrics
Events / Rules	React to AWS resource changes
Insights	Analyze logs using queries
🧠 How CloudWatch Works

AWS services send metrics to CloudWatch

CloudWatch evaluates metrics against alarms

Alarms trigger actions such as:

SNS notification

Auto Scaling

Lambda execution

🧪 LAB 1 – Monitor EC2 with CloudWatch
🪜 Steps:

Launch an EC2 instance

Enable Detailed Monitoring

Go to CloudWatch → Metrics → EC2

Select your instance ID

Choose CPUUtilization

Generate CPU Load
sudo yum install stress -y
stress --cpu 20 --timeout 300


📈 CPU utilization increases in CloudWatch graph.

⚙️ CloudWatch Alarms
🧠 What is an Alarm?

A CloudWatch Alarm monitors a metric and performs an action when a threshold is crossed.

📊 Alarm States
State	Description
OK	Normal condition
ALARM	Threshold breached
INSUFFICIENT_DATA	Not enough data
🧪 LAB 2 – Create a CloudWatch Alarm
Steps:

Go to CloudWatch → Alarms → Create Alarm

Choose metric → EC2 → CPUUtilization

Set condition:

CPU > 80%

Select action (optional SNS)

Create alarm

Test:
stress --cpu 20 --timeout 300


✅ Alarm enters ALARM state.

⚙️ Simple Scaling Policy
Concept:

One alarm = one scaling action.

Example:

CPU > 80% → Add 1 instance

CPU < 40% → Remove 1 instance

🧪 LAB 3 – Simple Scaling

Create Launch Template

Create Auto Scaling Group

Add Dynamic Scaling Policy

Attach CloudWatch alarm

Trigger load → Instance scales automatically.

⚙️ Step Scaling Policy
Step-based Scaling Example:
CPU Usage	Action
> 60%	Add 1 instance
> 80%	Add 2 instances
< 40%	Remove 1 instance
🧪 LAB 4 – Step Scaling

Create ASG

Create CloudWatch Alarm

Define scaling steps

Apply scaling policy

✅ Gradual scaling based on load.

📊 CloudWatch Dashboards
🧠 What is a Dashboard?

A visual panel displaying multiple metrics in one place.

You can monitor:

CPU Utilization

Network In / Out

Disk I/O

ALB request count

Alarm status

🧪 LAB 5 – Create Dashboard

Go to CloudWatch → Dashboards

Click Create Dashboard

Add widgets:

Line graph

Number

Text

Select EC2 metrics

Save dashboard

✅ Live metrics view enabled.

📉 CloudWatch Logs
What are Logs?

CloudWatch Logs collect:

Application logs

System logs

Custom logs

Example Sources:

EC2 logs

Lambda logs

VPC Flow Logs

Application logs

🧪 LAB 6 – View Logs

Go to CloudWatch → Logs

Select Log Group

Open log stream

View real-time logs

📈 CloudWatch Log Insights

Powerful query language for analyzing logs.

Example Query:
fields @timestamp, @message
| sort @timestamp desc
| limit 20

🔔 CloudWatch + SNS Integration
Flow:

Metric → Alarm → SNS → Email/SMS

Example:

CPU > 80%

SNS sends email alert

🧪 LAB 7 – Alarm with SNS Notification

Create SNS Topic

Subscribe Email

Attach SNS to Alarm

Confirm email subscription

✅ Alerts sent instantly.

☁️ CloudWatch + Auto Scaling
Auto Scaling uses:

CloudWatch metrics

Scaling policies

Health checks

Common Metrics:

CPUUtilization

ALBRequestCount

NetworkIn/Out

🧪 LAB 8 – Auto Scaling with CloudWatch

Create Launch Template

Create ASG

Add Target Tracking Policy:

CPU = 50%

Generate load

✅ ASG auto scales.

📊 CloudWatch + Grafana
Why Grafana?

Better visualization

Multiple data sources

Advanced dashboards

🧪 LAB 9 – Grafana Setup
Install Grafana:
sudo yum install grafana -y
sudo systemctl start grafana-server
sudo systemctl enable grafana-server


Access:

http://<EC2-IP>:3000


Default login:

admin / admin

Connect CloudWatch to Grafana

Add Data Source → CloudWatch

Provide IAM credentials

Attach IAM Role:

CloudWatchFullAccess

Create Dashboard:

CPU Utilization

Network Traffic

Disk I/O

✅ Live monitoring enabled.

🧠 Interview Questions (CloudWatch)
🔹 Basic

What is CloudWatch?

Difference between Metrics and Logs?

What is an Alarm?

What is a Namespace?

What is Detailed Monitoring?

🔹 Intermediate

Difference between Basic & Detailed monitoring?

How does CloudWatch trigger Auto Scaling?

What is a composite alarm?

What is a custom metric?

What is CloudWatch Logs Insights?

🔹 Advanced

Difference between Step Scaling & Target Tracking?

What happens when an alarm goes into INSUFFICIENT_DATA?

How does CloudWatch integrate with SNS?

Can CloudWatch monitor on-prem servers?

How does anomaly detection work?

🧠 Summary

You now understand:

✅ CloudWatch metrics, logs, alarms
✅ Monitoring EC2 & Auto Scaling
✅ Scaling policies
✅ Dashboards & visualization
✅ Log analysis
✅ SNS alerts
✅ Grafana integration


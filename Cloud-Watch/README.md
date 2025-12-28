☁️ Amazon CloudWatch – Zero to Hero
🔍 What is Amazon CloudWatch?

Amazon CloudWatch is a monitoring and observability service that provides visibility into AWS resources, applications, and services.

It helps you:

Monitor performance

Detect issues

Trigger automated actions

Visualize metrics and logs

⚙️ Core Features

Metrics – Monitor CPU, memory, disk, and network usage

Logs – Collect and analyze system and application logs

Alarms – Trigger actions based on thresholds

Dashboards – Visualize metrics in real time

Events / Rules – Automate actions based on changes

🧠 How CloudWatch Works

AWS services send metrics to CloudWatch

CloudWatch stores and analyzes data

Alarms monitor thresholds

Actions are triggered automatically

Example:

CPU > 80%

Alarm triggers

Auto Scaling adds a new EC2

🧪 LAB 1 – Monitor EC2 Using CloudWatch
Step 1: Launch EC2

Launch Amazon Linux EC2

Enable Detailed Monitoring

Step 2: View Metrics

Go to:

CloudWatch → Metrics → EC2 → Per-Instance Metrics


Select:

CPUUtilization

Step 3: Generate CPU Load
sudo yum install stress -y
stress --cpu 20 --timeout 300


✅ CPU usage increases in CloudWatch graph.

🚨 CloudWatch Alarms
What is an Alarm?

A CloudWatch Alarm monitors a metric and performs actions when thresholds are crossed.

Alarm States
State	Description
OK	Everything is normal
ALARM	Threshold crossed
INSUFFICIENT_DATA	Not enough data
🧪 LAB 2 – Create Alarm for EC2
Steps:

Go to CloudWatch → Alarms

Choose Create Alarm

Select metric → CPUUtilization

Set threshold → > 80%

Select action (SNS or Auto Scaling)

Create alarm

Test:
stress --cpu 20 --timeout 300


✅ Alarm triggers when threshold is crossed.

⚙️ Auto Scaling with CloudWatch

CloudWatch integrates with Auto Scaling to automatically increase or decrease EC2 instances.

Example:

CPU > 70% → Add instance

CPU < 40% → Remove instance

🧪 LAB 3 – Target Tracking Scaling Policy
Steps:

Create Launch Template

Create Auto Scaling Group

Choose Target Tracking Policy

Set target CPU → 50%

✅ ASG automatically scales instances.

🪜 Step Scaling Policy

Step scaling adds/removes instances gradually.

CPU Usage	Action
> 60%	Add 1 instance
> 80%	Add 2 instances
< 40%	Remove 1 instance
📊 CloudWatch Dashboards
What is a Dashboard?

A visual representation of metrics in one place.

Common Metrics:

CPUUtilization

NetworkIn / NetworkOut

DiskReadOps / DiskWriteOps

🧪 LAB 4 – Create Dashboard

Go to CloudWatch → Dashboards

Click Create dashboard

Add widgets:

CPUUtilization

NetworkIn

NetworkOut

Save dashboard

✅ Real-time visualization enabled.

📈 CloudWatch + Grafana Integration

Grafana provides advanced visualization for CloudWatch metrics.

🧪 LAB 5 – Install Grafana
sudo yum install grafana -y
sudo systemctl start grafana-server
sudo systemctl enable grafana-server


Access:

http://<EC2-Public-IP>:3000


Login:

Username: admin
Password: admin

Connect Grafana to CloudWatch

Go to Connections → Data Sources

Select Amazon CloudWatch

Attach IAM role with:

CloudWatchFullAccess

Create Dashboard

Add metrics:

CPUUtilization

NetworkIn / NetworkOut

Disk I/O

✅ Live visualization enabled.

📊 CloudWatch vs CloudTrail
Feature	CloudWatch	CloudTrail
Purpose	Monitoring	Auditing
Tracks	Metrics & Logs	API calls
Use Case	Performance	Security & Compliance
🧠 CloudWatch Interview Questions
1. What is CloudWatch?

Monitoring service for AWS resources and applications.

2. What metrics does CloudWatch track?

CPU, memory, disk, network, custom metrics.

3. What is a CloudWatch Alarm?

Triggers action when threshold is breached.

4. What is detailed monitoring?

1-minute metric granularity.

5. What is a custom metric?

User-defined metric sent to CloudWatch.

6. Difference between Logs and Metrics?

Logs = text events
Metrics = numerical values

7. What is anomaly detection?

ML-based automatic anomaly detection.

8. Can CloudWatch trigger Auto Scaling?

Yes.

9. What is EventBridge?

Event-driven automation service.

10. What is CloudWatch Agent?

Agent used to collect logs and system metrics.

🧠 Advanced CloudWatch Concepts
11. What is a Composite Alarm?

Combines multiple alarms using AND/OR logic.

12. Can CloudWatch monitor on-prem servers?

Yes, using CloudWatch Agent.

13. What is metric resolution?

Standard: 5 min
Detailed: 1 min

14. What is Contributor Insights?

Identifies top contributors to a metric.

15. What is CloudWatch Synthetics?

Monitors endpoints using canaries.

✅ Final Summary

You now understand:

✔ CloudWatch Metrics
✔ Logs & Alarms
✔ Auto Scaling Integration
✔ Dashboards
✔ Grafana Monitoring
✔ Interview-Level Concepts


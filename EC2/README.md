
---

# 🚀 Amazon EC2 – Zero to Hero

## 💡 What is Amazon EC2?

Amazon EC2 (**Elastic Compute Cloud**) is a web service that provides scalable, on-demand virtual servers in the AWS Cloud (**instances**).

* Launch servers in minutes
* Scale up/down based on traffic
* Pay only for what you use

---

## 🧱 EC2 Basics

* **Instance** → Virtual server in AWS
* **AMI (Amazon Machine Image)** → Template with OS + software
* **Instance Type** → Hardware (CPU, RAM, network performance)
* **EBS Volume** → Disk attached to instance
* **Security Group** → Virtual firewall
* **Key Pair** → SSH/RDP login credentials

---

## 🧪 LAB 1 – Launch & Connect to an EC2 (Amazon Linux)

1. Go to **EC2 → Instances → Launch Instance**
2. Choose AMI: **Amazon Linux 2**
3. Choose Instance Type: `t2.micro` (Free tier)
4. Create / select **Key Pair** (for SSH)
5. Configure **Security Group**:

   * Allow **SSH (22)** from your IP
6. Launch instance
7. Connect via terminal:

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

---

## 🌐 Install Apache HTTP Server (Amazon Linux)

Default web directory:

```bash
/var/www/html
```

### 🧪 LAB 2 – Install & Test Apache

```bash
# Become root
sudo su

# Install Apache
yum install httpd -y

# Check status
systemctl status httpd

# If inactive, start it
systemctl start httpd

# Enable on boot
systemctl enable httpd
```

Now:

* Open browser → `http://<EC2-Public-IP>`
* If not loading:

  * Go to **Security Group → Inbound Rules**
  * Add: **HTTP (Port 80) – 0.0.0.0/0**

To remove Apache:

```bash
yum remove httpd -y
```

---

## 🎨 Host a Custom Website (Apache)

### 🧪 LAB 3 – Simple HTML Page

```bash
# Go to default web folder
cd /var/www/html

# Create index.html
sudo vi index.html
```

Paste some HTML:

```html
<h1>Hello from EC2 Apache!</h1>
<p>Deployed on Amazon Linux</p>
```

Save (`:wq`), then open:

```text
http://<EC2-Public-IP>
```

Your page should load 🎉

---

## 🎨 Use a Custom HTML/CSS Template (Apache)

### 🧪 LAB 4 – Import Template

```bash
cd /var/www/html

# Install unzip if needed
sudo yum install unzip -y

# Download template
wget <link-to-template.zip>

# Unzip
unzip <file.zip>

# Move content to web root (optional)
mv * /var/www/html
```

Then open `http://<EC2-Public-IP>` → template website should appear.

---

## ⚙️ NGINX on EC2 (Amazon Linux)

**Nginx** is a high-performance web server, reverse proxy and load balancer.

Default directory (Amazon Linux):

```bash
/usr/share/nginx/html
```

### 🧪 LAB 5 – Install NGINX

```bash
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

Test:

```text
http://<EC2-Public-IP>
```

### 🧪 LAB 6 – Custom Website with NGINX

```bash
cd /usr/share/nginx/html
sudo rm -rf *

sudo vi index.html
```

Add HTML → Save → Refresh browser.

---

## 🐧 EC2 with Ubuntu

* Default user: `ubuntu`
* Package manager: `apt`

### Apache on Ubuntu

```bash
sudo apt update
sudo apt install apache2 unzip -y

cd /var/www/html
```

### NGINX on Ubuntu

```bash
sudo apt update
sudo apt install nginx -y

# Default web root usually:
cd /var/www/html
```

---

## 🔁 Proxy Concepts

### 🔹 Forward Proxy

* Sits **in front of clients**
* Client → Proxy → Internet
* Used for filtering, caching, monitoring

### 🔹 Reverse Proxy

* Sits **in front of servers**
* Client → Reverse Proxy → Backend
* Used for load balancing, SSL offloading, hiding internal structure

NGINX is often used as a **reverse proxy**.

---

## 🪟 EC2 (Windows) + IIS Web Server

### 🧪 LAB 7 – Connect to Windows Instance

1. Launch Windows AMI (e.g., Windows Server)
2. Allow **RDP (3389)** in Security Group
3. Select instance → **Connect → RDP Client**
4. Upload **Key Pair** → Decrypt password
5. Download RDP file and open in Remote Desktop app
6. Use decrypted password to log in

### 🧪 LAB 8 – Install IIS

1. Open **Server Manager**
2. **Add Roles and Features**
3. Select **Web Server (IIS)**
4. Complete installation
5. Open browser inside server, go to `http://localhost` or from outside using EC2 public IP

Default web root:

```text
C:\inetpub\wwwroot
```

Create `index.html` there.

---

## 🧱 AMI – Amazon Machine Image

### What is AMI?

A template to launch instances with:

* OS
* Pre-installed software
* Configurations
* Attached volumes

### Types of AMIs

* AWS Provided
* Marketplace
* Custom (created from your own instance)

### 🧪 LAB 9 – Create Custom AMI

1. Configure an EC2 instance the way you like
2. EC2 → Instances → Select → **Actions → Images and Templates → Create Image**
3. Give name & description
4. Check in **EC2 → AMIs**
5. Launch new instances from your custom AMI

---

## 💾 Block Storage – EBS & Instance Store

### 1️⃣ Instance Store (Ephemeral)

* Physically attached to host
* Very fast, **BUT** data lost when instance stops/terminates
* Good for cache / temporary data

### 2️⃣ EBS – Elastic Block Store

* Network-attached persistent storage
* Data survives stop/start and reboots
* Can attach/detach between instances (same AZ)
* Supports **snapshots**

### ⚙️ EBS Volume Types (Common)

**SSD (general):**

* `gp2` / `gp3` → General purpose, boot volumes

**SSD (IOPS-optimized):**

* `io1` / `io2` → High IOPS, databases

**HDD:**

* `st1` → Throughput-optimized (big data)
* `sc1` → Cold, cheap storage

---

## 🧪 LAB 10 – Attach & Mount New EBS Volume

1. EC2 → **Elastic Block Store → Volumes → Create Volume**
2. Choose size, type, AZ (same as instance)
3. **Actions → Attach Volume** to your instance
4. Connect via SSH:

```bash
lsblk          # See new disk, e.g., /dev/xvdb

sudo file -s /dev/xvdb
sudo mkfs -t ext4 /dev/xvdb     # Format

sudo mkdir /mnt/data
sudo mount /dev/xvdb /mnt/data

lsblk          # Confirm mount
```

### Permanent Mount (fstab)

```bash
sudo vi /etc/fstab
```

Add line:

```text
/dev/xvdb /mnt/data ext4 defaults,nofail 0 2
```

Now it remounts on reboot ✅

---

## 📸 EBS Snapshots

* Point-in-time backup of EBS volume
* Incremental
* Stored in S3 (managed by AWS)
* Can be used to:

  * Restore volume
  * Create new AMI
  * Copy across regions

### 🧪 LAB 11 – Snapshot & Restore

1. Select Volume → **Actions → Create Snapshot**
2. EC2 → **Snapshots**
3. Create Volume from Snapshot
4. Attach to instance and mount:

```bash
sudo mount -t xfs -o nouuid /dev/xvdb1 /mnt/mount
```

---

## ⚙️ EC2 Instance Types

Format:

```text
<family><generation><features>.<size>
```

Examples:

* `t2.micro` → General purpose, small
* `c5.large` → Compute optimized
* `r5.xlarge` → Memory optimized
* `i3.2xlarge` → Storage optimized
* `g4dn.4xlarge` → GPU for ML/graphics

---

## 💸 EC2 Purchasing Options

| Option                | Description                                    | Use Case                           |
| --------------------- | ---------------------------------------------- | ---------------------------------- |
| On-Demand             | Pay per second/hour                            | Unpredictable workloads            |
| Reserved Instances    | 1 or 3-year commitment, big discount           | Stable workloads                   |
| Savings Plans         | Commit $/hour, flexible services               | Long-term but flexible usage       |
| Spot Instances        | Unused capacity, up to ~90% cheaper, interrupt | Batch, stateless, fault-tolerant   |
| Dedicated Host        | Physical server for you                        | Compliance / licensing             |
| Dedicated Instances   | Isolated hardware                              | Isolation needed                   |
| Capacity Reservations | Reserve capacity in an AZ                      | Guaranteed capacity in specific AZ |

---

## 🔐 Key Pair & Lost Key Recovery

Key Pair = **public + private key**

* Public key stored in instance
* Private key with you

Authorized keys location (Amazon Linux):

```bash
/home/ec2-user/.ssh/authorized_keys
```

### 🧪 LAB 12 – Recover EC2 Using Dummy Server (Lost Key)

1. Stop the main instance
2. Detach its root EBS volume
3. Attach that volume to a **dummy EC2** (with working key)
4. On dummy instance:

```bash
lsblk

sudo mount -t xfs -o nouuid /dev/xvdb1 /mnt/mount

cd /mnt/mount/home/ec2-user/.ssh
```

5. Replace `authorized_keys` with dummy instance’s key:

```bash
sudo cp /home/ec2-user/.ssh/authorized_keys /mnt/mount/home/ec2-user/.ssh/authorized_keys
```

6. Detach the volume → Attach back to original instance as root
7. Start main instance → Now you can SSH with dummy key 🎯

---

## 🐚 Shell Script Automation on EC2

### 🧪 LAB 13 – Script to Install Apache & Website

```bash
vi web.sh
```

Add:

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "<h1>Hi From Website</h1>" > /var/www/html/index.html
```

Then:

```bash
chmod 700 web.sh
./web.sh
```

Apache installs & serves page automatically ✅

---

## ⚡ EC2 User Data (Bootstrapping)

**User Data** = Script that runs only on **first boot**.

### 🧪 LAB 14 – Use User Data for Apache

During instance launch → **Advanced Details → User data**:

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "<h1>Hi From Website</h1>" > /var/www/html/index.html
```

Instance comes up with Apache running & page ready 🎉

### Refreshing User Data

To force re-run:

```bash
sudo rm -rf /var/lib/cloud/*
sudo cloud-init init
sudo cloud-init modules --mode=config
sudo cloud-init modules --mode=final
```

---

## 🌐 ENI – Elastic Network Interface

ENI = virtual network card for EC2.

Contains:

* Private IP (mandatory)
* Optional Public / Elastic IP
* Security groups
* MAC address

### 🧪 LAB 15 – Attach Additional ENI

1. EC2 → **Network Interfaces → Create**
2. Choose VPC, subnet, security group
3. **Actions → Attach** to EC2
4. Optionally allocate & attach an **Elastic IP** to the ENI

Now instance can have multiple IPs.

---

## 🌍 Virtual Hosting (Two Websites on One EC2)

### 🧪 LAB 16 – Name/IP Based Virtual Hosting (Apache)

1. Create directories:

```bash
cd /var/www/html
sudo mkdir web1 web2
```

2. Add `index.html` inside each:

```bash
sudo vi /var/www/html/web1/index.html
sudo vi /var/www/html/web2/index.html
```

3. Edit Apache config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Add:

```apache
<VirtualHost Private-IP1:80>
    DocumentRoot "/var/www/html/web1"
</VirtualHost>

<VirtualHost Private-IP2:80>
    DocumentRoot "/var/www/html/web2"
</VirtualHost>
```

4. Restart Apache:

```bash
sudo systemctl restart httpd
```

Now each IP shows a different site.

---

## 💻 PuTTY (Windows SSH Client)

Steps:

1. Convert `.pem` to `.ppk` using **PuTTYgen**
2. Open **PuTTY**

   * Host: EC2 Public IP
   * SSH → Auth → Browse .ppk
3. Connect → Accept → Logged in ✅

---

## 🔒 Session Manager (No SSH Needed)

**AWS Systems Manager – Session Manager** lets you connect **without**:

* SSH port 22
* Key pairs
* Public IP

### 🧪 LAB 17 – Use Session Manager

1. Ensure instance has **SSM Agent** (Amazon Linux/Ubuntu usually have it)
2. Create IAM Role with policy:

   * `AmazonSSMManagedInstanceCore`
3. Attach role to instance
4. EC2 → **Connect → Session Manager → Start Session**

You get terminal access from the browser 🔐

---

## 🖥️ EC2 Serial Console

* Serial-level access like a physical server
* Good for boot issues, network misconfig

Steps (once per account/region):

1. EC2 → **Settings → EC2 Serial Console → Enable**
2. Use supported Nitro-based instances
3. EC2 → Select instance → **Connect → EC2 Serial Console**

---

## ⚙️ Burstable vs Fixed Performance Instances

### 🔹 Burstable

* Families: `t2`, `t3`, `t3a`, `t4g`
* Use **CPU credits**
* Cheap, good for variable workloads

### 🔹 Fixed Performance

* Families: `m5`, `m6i`, `c5`, `r5`, etc.
* Constant, predictable performance

---

## ⚖️ Load Balancers (ELB)

A **Load Balancer** distributes traffic across multiple instances.

Types:

1. **ALB** – Application (Layer 7)
2. **NLB** – Network (Layer 4)
3. **Gateway LB** – For firewalls/inspection
4. **Classic LB** – Old, legacy

### 🔁 Basic Flow

```text
User → Load Balancer → Target Group → EC2 instances
```

---

## 🧪 LAB 18 – Create Application Load Balancer (ALB)

1. Launch **2 EC2 instances** with Apache pages:

   * Instance 1: "Server 1"
   * Instance 2: "Server 2"
2. EC2 → **Target Groups → Create**

   * Type: Instances
   * Register both EC2s
3. EC2 → **Load Balancers → Create**

   * Type: Application Load Balancer
   * Internet-facing
   * Listener: HTTP 80
   * Select subnets & SG (allow HTTP)
   * Attach Target Group
4. Copy ALB **DNS name** → open in browser

   * Refresh multiple times → see traffic switching between servers

---

## ⚙️ Auto Scaling Group (ASG)

Auto Scaling automatically adjusts number of EC2 instances.

Key settings:

* **Min** → Minimum instances
* **Max** → Max instances
* **Desired** → Normal target count

Requires **Launch Template (LT)**:

Includes:

* AMI
* Instance type
* Key pair
* Security group
* User data
* Storage & IAM role

---

## 🧪 LAB 19 – Create Auto Scaling Group

1. EC2 → **Launch Templates → Create Template**
2. EC2 → **Auto Scaling Groups → Create**

   * Choose launch template
   * Choose subnets
   * Configure Min/Max/Desired (e.g., 1/3/1)
3. Finish creation
4. Stop 1 instance → ASG will launch a new one automatically 🔁

---

## 🧪 LAB 20 – Integrate ASG with ALB

1. Create an ALB + Target Group (from earlier lab)
2. In ASG:

   * Attach the same Target Group
3. Ensure SGs allow traffic from ALB to instances
4. Now:

   * Users hit ALB DNS
   * ALB routes → ASG instances
   * ASG scales instances, ALB distributes

---

## 📊 CloudWatch & Auto Scaling Policies

**CloudWatch** monitors:

* CPU, memory (via agent), disk, network, etc.

**Alarms** → Trigger when metric crosses thresholds.

Alarm states:

* `OK`
* `ALARM`
* `INSUFFICIENT_DATA`

---

## 🧪 LAB 21 – CloudWatch Alarm for EC2 CPU

1. Launch instance (enable **Detailed Monitoring** if needed)
2. CloudWatch → **Metrics → EC2 → Per-Instance Metrics**
3. Select `CPUUtilization` for instance
4. Create alarm:

   * Condition: `CPUUtilization > 80% for 5 minutes`
5. Use `stress` to increase load:

```bash
sudo yum install stress -y
stress --cpu 20 --timeout 300
```

CPU goes high → Alarm becomes `ALARM`.

---

## 🧪 LAB 22 – Target Tracking Policy for ASG

1. Open your **Auto Scaling Group**
2. Go to **Automatic Scaling → Add Policy**
3. Choose **Target Tracking**

   * Metric: Average CPUUtilization
   * Target: e.g., 50%
4. When CPU > 50% → ASG adds instances
   When CPU < 50% → ASG terminates extra ones

Use `stress` on one instance, watch new instances launched automatically.

---

## 🏁 EC2 Summary (Interview Ready)


## ✅ **1. What is Amazon EC2?**

EC2 is a virtual server service that allows users to launch, manage, and scale compute resources in AWS.

---

## ✅ **2. What is an AMI?**

AMI (Amazon Machine Image) is a template used to launch EC2 instances.
It includes OS, software, and configuration.

---

## ✅ **3. Difference between Stop vs Terminate an instance**

| Action        | What Happens                                                     |
| ------------- | ---------------------------------------------------------------- |
| **Stop**      | Instance shuts down, EBS volume saved, billing for EBS continues |
| **Terminate** | Instance deleted, EBS deleted if "delete-on-termination=true"    |

---

## ✅ **4. What is a Key Pair?**

Used for SSH (Linux) or RDP (Windows) login.
Contains **public + private key**.

---

## ✅ **5. Public IP vs Elastic IP**

| Public IP             | Elastic IP              |
| --------------------- | ----------------------- |
| Auto-assigned         | Static, you manage it   |
| Changes on stop/start | Remains same            |
| Free                  | Charged if not attached |

---

## ✅ **6. What is Instance Store?**

Temporary high-speed disk attached physically to host.
⚠️ *Data is lost when instance stops or terminates.*

---

## ✅ **7. What is EBS?**

EBS = network-attached persistent storage.
Survives stop/start.

---

## ✅ **8. What is User Data?**

A script that runs at **first boot** for automation (install apache, updates etc.)

---

## ✅ **9. What is ENI?**

Elastic Network Interface:
Private IPs, MAC address, security groups attached to EC2.

---

## ✅ **10. What is a Security Group?**

A **stateful** firewall that allows/denies traffic *to the instance*.

---

## 🧠 **Advanced EC2 Questions**

## ⭐ **11. What happens to root volume on termination?**

If "DeleteOnTermination=true", root EBS is deleted automatically.

---

## ⭐ **12. What is Placement Group? Types?**

1. **Cluster** → Low latency, high network throughput
2. **Spread** → Instances placed across racks for HA
3. **Partition** → Used for Hadoop, Kafka, big distributed apps

---

## ⭐ **13. What are EBS volume types?**

* gp3 / gp2 → General Purpose
* io1 / io2 → High IOPS
* st1 → Throughput optimized
* sc1 → Cold HDD

---

## ⭐ **14. What is a Launch Template?**

A saved configuration for EC2 containing AMI, instance type, SG, user data.
Used by ASG & Spot Fleets.

---

## ⭐ **15. How do you recover EC2 when Key Pair is lost?**

Detach root volume → Attach to temporary instance → Replace **authorized_keys** → Attach back.

---

## ⭐ **16. What is the difference between On-Demand, Spot, and Reserved?**

* **On-Demand** – pay per hour, flexible
* **Reserved** – 1/3 year contract, cheaper
* **Spot** – 90% cheaper, can be interrupted anytime

---

## ⭐ **17. What is EC2 Hibernate?**

Preserves RAM to disk → instance resumes from same state.

---

## ⭐ **18. What is Dedicated Host?**

A physical server dedicated to one customer. Used for compliance.

---

## ⭐ **19. What is Spot Fleet?**

A group of spot instances managed automatically.

---

## ⭐ **20. How do you monitor EC2?**

Using CloudWatch for CPU, disk, memory (with agent).

---

---

# ⚖️ Load Balancer (ELB) Interview Questions

## ✅ **1. What is a Load Balancer?**

Distributes traffic across multiple EC2 instances.

---

## ✅ **2. Types of AWS Load Balancers**

1. **ALB — Application Load Balancer** (Layer 7)
2. **NLB — Network Load Balancer** (Layer 4)
3. **CLB — Classic Load Balancer** (Old)
4. **Gateway Load Balancer** (Layer 3 firewall/inspection)

---

## ✅ **3. What is a Target Group?**

A collection of EC2 / IPs / Lambdas behind an LB.

---

## ✅ **4. What are Health Checks?**

LB periodically checks instance health.
Unhealthy instances do NOT receive traffic.

---

## ✅ **5. ALB vs NLB**

| ALB                    | NLB                         |
| ---------------------- | --------------------------- |
| Layer 7                | Layer 4                     |
| URL-based routing      | Millions of requests/second |
| Supports WebSockets    | Very high performance       |
| Good for microservices | Good for real-time traffic  |

---

## ✅ **6. What is Sticky Session?**

LB sends user repeatedly to the same instance.

---

## ✅ **7. Can ALB route by path or hostname?**

Yes.
Examples:

* /api → API servers
* /images → image servers

---

## ✅ **8. What is Cross-Zone Load Balancing?**

LB distributes traffic evenly across all AZs.

---

---

# 🚀 Auto Scaling Group (ASG) Interview Questions

## ✅ **1. What is Auto Scaling?**

Automatically increases or decreases EC2 instances based on load.

---

## ✅ **2. What is Launch Template?**

Configuration used by ASG to launch EC2.

---

## ✅ **3. What are ASG Capacity Settings?**

* **Min** – minimum instances always running
* **Desired** – normal state
* **Max** – maximum allowed

---

## ✅ **4. Types of Scaling Policies**

1. **Target Tracking** → Keep CPU at 50%
2. **Simple Policy** → Trigger when CPU > 80%
3. **Step Scaling** → Add 1 instance at 60%, 2 instances at 80%
4. **Scheduled Scaling** → Scale at specific time

---

## ✅ **5. What happens if an instance fails?**

ASG automatically replaces it.

---

## ✅ **6. How ASG works with ALB?**

* ALB sends traffic
* ASG keeps required instances alive
* ALB health checks failing → ASG replaces instance

---

## ✅ **7. Does ASG work in multiple AZs?**

Yes. Best practice is **multi-AZ ASG** for HA.

---

## ✅ **8. What is Warm Pool?**

A pool of pre-initialized instances ready to start quickly.

---

## ✅ **9. Does Auto Scaling reduce cost?**

Yes. Removes unused instances when demand is low.

---

## 🎯 **10. What is Lifecycle Hook?**

Allows custom actions (like install software) before instance enters service.

---

---



## By now, you understand:

✅ What is EC2, AMI, EBS, SG, Key Pair
✅ Apache & NGINX on Amazon Linux & Ubuntu
✅ Windows EC2 + IIS
✅ EBS volumes, mounts, snapshots & backup
✅ Instance store vs EBS
✅ Instance families & purchasing options
✅ User Data & shell scripting automation
✅ Lost key recovery using dummy server
✅ ENIs & virtual hosting
✅ PuTTY, Session Manager, Serial Console
✅ Load Balancers (ALB/NLB) and Target Groups
✅ Auto Scaling Groups, Launch Templates
✅ CloudWatch metrics & alarms, scaling policies

�


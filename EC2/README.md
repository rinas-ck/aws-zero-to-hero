
---

# 🖥️ Amazon EC2 — Zero to Hero

A complete, practical guide to **Amazon EC2** for AWS & DevOps interviews — with **theory + labs** in one place.

---

## 💡 1. What is Amazon EC2?

**Amazon EC2 (Elastic Compute Cloud)** is a web service that provides scalable virtual servers (called **instances**) in the AWS Cloud.

* Run applications on-demand
* Scale up/down easily
* Pay only for what you use

When you launch an EC2 instance, you choose:

* **AMI** → OS image (Amazon Linux, Ubuntu, Windows, etc.)
* **Instance Type** → CPU, RAM, network performance
* **Storage** → EBS volumes / Instance store
* **Network** → VPC, Subnet, Security Group
* **Key Pair** → SSH / RDP access

---

## 🧱 2. Lab: Install Apache on Amazon Linux (Web Server)

### 🧪 Lab 1 — Install Apache on EC2 (Amazon Linux)

**Goal:** Launch an EC2 instance and install Apache HTTP Server.

1️⃣ **Launch an EC2 Instance**

* AMI: **Amazon Linux 2**
* Instance Type: `t2.micro`
* Network:

  * VPC + Public Subnet
  * Auto-assign Public IP: **Enable**
* Security Group:

  * Allow **SSH (22)** from your IP
  * Allow **HTTP (80)** from `0.0.0.0/0`

2️⃣ **Connect to Instance (SSH)**

```bash
ssh -i mykey.pem ec2-user@<PUBLIC-IP>
```

3️⃣ **Become root**

```bash
sudo su
```

4️⃣ **Install Apache**

```bash
yum install httpd -y
```

5️⃣ **Check Apache status**

```bash
systemctl status httpd
```

6️⃣ **Start Apache if inactive**

```bash
systemctl start httpd
systemctl enable httpd   # start on reboot
```

7️⃣ **Test from Browser**

* Copy EC2 **Public IP**
* Open: `http://<PUBLIC-IP>`

If it doesn’t work → Security Group must allow **HTTP (80)**.

8️⃣ **Uninstall Apache (optional)**

```bash
yum remove httpd -y
```

📂 **Default web directory:**

```text
/var/www/html
```

---

## 🎨 3. Host a Custom Website (Apache + HTML + Template)

### 🧪 Lab 2 — Simple Custom Website

1️⃣ **Go to web directory**

```bash
cd /var/www/html
```

2️⃣ **Create home page**

```bash
sudo vi index.html
```

Example content:

```html
<h1>Welcome to My EC2 Website</h1>
<p>Deployed on Apache HTTP Server</p>
```

3️⃣ **Save & exit** (`:wq`)

4️⃣ **Access in browser**

```text
http://<EC2-PUBLIC-IP>
```

---

### 🧪 Lab 3 — Use External HTML/CSS Template

1️⃣ **Install `wget` and `unzip`**

```bash
yum install wget unzip -y
```

2️⃣ **Download template**

```bash
wget <link-to-template.zip>
```

3️⃣ **Unzip**

```bash
unzip template.zip
```

4️⃣ **Move files to web root**

```bash
mv * /var/www/html
```

5️⃣ **Open browser**

* Visit `http://<PUBLIC-IP>`
* You should see your **CSS-based template**.

---

## ⚙️ 4. Nginx on Amazon Linux

**Nginx** is a high-performance web server and reverse proxy.

📂 Default directory (Amazon Linux):

```text
/usr/share/nginx/html
```

### 🧪 Lab 4 — Install and Run Nginx

```bash
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

Visit: `http://<PUBLIC-IP>`

---

### 🧪 Lab 5 — Custom Site with Nginx

1️⃣ **Go to directory**

```bash
cd /usr/share/nginx/html
sudo rm -rf *
```

2️⃣ **Create page**

```bash
sudo vi index.html
```

```html
<h1>Hello from Nginx on EC2!</h1>
```

3️⃣ **Save & test in browser**

---

## 🐧 5. EC2 with Ubuntu (Apache + Nginx)

### 🧪 Lab 6 — Apache on Ubuntu

User: `ubuntu`
Package manager: `apt`

```bash
sudo apt update
sudo apt install apache2 unzip -y
cd /var/www/html
```

Same steps: edit `index.html`, browse to `http://<PUBLIC-IP>`.

### 🧪 Lab 7 — Nginx on Ubuntu

```bash
sudo apt update
sudo apt install nginx -y
```

Default web root usually:

```text
/var/www/html
```

---

## 🪟 6. Windows EC2 + IIS Web Server

### 🧪 Lab 8 — Windows Server + IIS

1️⃣ Launch a **Windows Server** EC2 instance.
2️⃣ Connect using **RDP**:

* EC2 Console → Select Instance → **Connect**
* Go to **RDP Client**
* Upload `.pem` Key Pair → Decrypt password
* Download RDP file → Open → Paste password.

3️⃣ Install **IIS**:

* Open **Server Manager**
* Choose **Add Roles and Features**
* Select **Web Server (IIS)** → Install.

4️⃣ Test:

* Open browser inside or outside
* Go to `http://<PUBLIC-IP>`

Default path:

```text
C:\inetpub\wwwroot
```

Create or edit:

```text
index.html
```

---

## 🧩 7. AMI (Amazon Machine Image)

### 📘 What is an AMI?

A template used to launch EC2 instances with:

* OS
* Installed software
* Configuration
* Attached volumes

Types:

* **AWS-provided AMIs**
* **Marketplace AMIs**
* **Custom AMIs** (created by you)

---

### 🧪 Lab 9 — Create Custom AMI from EC2

1️⃣ Setup EC2 (install Apache, app, tools).
2️⃣ EC2 Console → Select Instance →
**Actions → Image and templates → Create Image**.
3️⃣ Give name → Create.
4️⃣ Check under **Images → AMIs**.
5️⃣ Launch new instances from this AMI anytime.

---

## 💾 8. EC2 Storage: Instance Store vs EBS

### 📦 1. Instance Store (Ephemeral)

* Physically attached to the host
* Very fast, **temporary**
* Data lost if instance stops/terminates
* Good for **cache** or temporary data

### 📦 2. EBS (Elastic Block Store)

* Network-attached, **persistent**
* Survives instance stop/start
* Can be detached/attached to instances
* Supports **snapshots**

### ⚙️ EBS Volume Types (High-Level)

| Type  | Category | Use Case                        |
| ----- | -------- | ------------------------------- |
| gp2/3 | SSD      | General workloads, boot volumes |
| io1/2 | SSD      | High IOPS, databases            |
| st1   | HDD      | Big data, streaming             |
| sc1   | HDD      | Cold, infrequent access         |

---

## 🧪 Lab 10 — Attach and Mount New EBS Volume (Linux)

1️⃣ **Create Volume**

* EC2 → **Elastic Block Store → Volumes**
* Create volume in same **AZ** as instance
* Attach to instance (e.g. `/dev/xvdb`)

2️⃣ **Check disk**

```bash
lsblk
sudo file -s /dev/xvdb
```

3️⃣ **Create filesystem**

```bash
sudo mkfs -t ext4 /dev/xvdb
```

4️⃣ **Create mount point & mount**

```bash
sudo mkdir /mnt/data
sudo mount /dev/xvdb /mnt/data
```

5️⃣ **Verify**

```bash
lsblk
df -h
```

6️⃣ **Make it persistent (on reboot)**

Edit `/etc/fstab`:

```text
/dev/xvdb /mnt/data ext4 defaults,nofail 0 2
```

---

## 📸 9. EBS Snapshots

* **Point-in-time** backup of EBS volume
* Stored in S3 (managed by AWS)
* **Incremental** (only changed blocks)
* Used to:

  * Restore data
  * Create new volumes
  * Create AMIs

### 🧪 Lab 11 — Create Snapshot & Restore

1️⃣ EC2 → Volumes → Select Volume → **Create Snapshot**.
2️⃣ Later → Snapshots → **Create Volume from Snapshot**.
3️⃣ Attach new volume to EC2 & mount.

---

## ⚙️ 10. EC2 Instance Types & Naming

### 🧠 Instance Families

* **General Purpose:** `t`, `m` (balanced CPU/RAM)
* **Compute Optimized:** `c` (high CPU)
* **Memory Optimized:** `r`, `x` (databases, in-memory)
* **Storage Optimized:** `i`, `d` (high I/O)
* **Accelerated Computing:** `g`, `p` (GPU)

### 🔤 Naming Convention

Format:

```text
<family><generation><features>.<size>
```

Examples:

* `t2.micro` → General, gen2, micro
* `c5.large` → Compute, gen5, large
* `r6g.xlarge` → Memory, gen6, Graviton

---

## 💸 11. EC2 Purchasing Options

| Option                  | Description                           | Best For                           |
| ----------------------- | ------------------------------------- | ---------------------------------- |
| On-Demand               | Pay per hour/second                   | Short-term, unpredictable workload |
| Reserved Instances (RI) | 1–3 year commit, big discount         | Stable long-term workloads         |
| Savings Plans           | Commit $/hr usage for 1–3 years       | Flexible usage types               |
| Spot Instances          | Use spare capacity, up to 90% cheaper | Fault-tolerant batch/CI jobs       |
| Dedicated Hosts         | Physical server reserved for you      | Compliance/licensing               |
| Dedicated Instances     | Isolated hardware (AWS-managed)       | Extra isolation                    |
| Capacity Reservations   | Reserve capacity in specific AZ       | Critical apps needing capacity     |

---

## 🔐 12. Key Pairs, SSH & Recovery

* **Key Pair:** Public key + Private key
* Private key: stays with you (`.pem`)
* Public key: stored on EC2 under:

```text
/home/ec2-user/.ssh/authorized_keys
```

### 🧪 Lab 12 — Lost Private Key Recovery (Dummy EC2 Trick)

**Goal:** Recover SSH access when `.pem` file is lost.

1️⃣ Stop **Main Instance** (lost key one).
2️⃣ Detach its root EBS volume.
3️⃣ Attach that volume to a **Dummy Instance**.
4️⃣ On dummy EC2:

```bash
sudo mkdir /mnt/main
sudo mount -t xfs -o nouuid /dev/xvdb1 /mnt/main
cd /mnt/main/home/ec2-user/.ssh
```

5️⃣ Replace `authorized_keys` with a new one (from dummy’s key pair).
6️⃣ Unmount & detach the volume:

```bash
cd /
sudo umount /mnt/main
```

7️⃣ Attach volume back to **Main Instance** as root.
8️⃣ Start main instance and connect using dummy’s `.pem`.

✅ Access restored.

---

### 🔑 Generate New SSH Key Pair (Local)

```bash
ssh-keygen -t rsa
```

* Private: `id_rsa`
* Public: `id_rsa.pub`

You can copy the **public key** into `authorized_keys` on EC2 to allow login.

---

## 🐚 13. Shell Scripting + User Data

### 🧪 Lab 13 — Simple Apache Install Script

Create script:

```bash
vi web.sh
```

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "<h1>Hi From Website</h1>" > /var/www/html/index.html
```

Make executable & run:

```bash
chmod 700 web.sh
./web.sh
```

---

### ⚡ User Data (Run Script at First Boot)

**User Data** = Script that runs automatically when instance is first launched.

Example (Apache auto setup):

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
echo "<h1>Hi From Website</h1>" > /var/www/html/index.html
```

Add this script in:

> Launch EC2 → Advanced details → User data

---

### 🔁 Re-run Updated User Data (Cloud-Init Reset)

On the instance:

```bash
sudo rm -rf /var/lib/cloud/*
sudo cloud-init init
sudo cloud-init modules --mode=config
sudo cloud-init modules --mode=final
```

---

## 🌐 14. ENI (Elastic Network Interface) & Virtual Hosting

### 🧠 ENI Basics

ENI = Virtual network card attached to an EC2 instance.

Includes:

* Private IP(s)
* Optional public / Elastic IP
* Security groups
* MAC address

### 🧪 Lab 14 — Attach Extra ENI + Elastic IP

1️⃣ EC2 → **Network Interfaces** → Create ENI
2️⃣ Choose subnet & security group
3️⃣ Attach ENI to an instance
4️⃣ Allocate **Elastic IP** → Associate to ENI

Result: Instance now has multiple private IPs & Elastic IP.

---

### 🌍 Name-Based Virtual Hosting (Apache)

Goal: Host 2 websites on one EC2.

1️⃣ Inside `/var/www/html`:

```bash
sudo mkdir web1 web2
echo "<h1>Site 1</h1>" | sudo tee /var/www/html/web1/index.html
echo "<h1>Site 2</h1>" | sudo tee /var/www/html/web2/index.html
```

2️⃣ Edit Apache config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Add:

```apache
<VirtualHost *:80>
    ServerName site1.example.com
    DocumentRoot "/var/www/html/web1"
</VirtualHost>

<VirtualHost *:80>
    ServerName site2.example.com
    DocumentRoot "/var/www/html/web2"
</VirtualHost>
```

3️⃣ Restart Apache:

```bash
sudo systemctl restart httpd
```

With proper DNS, both domains go to same EC2 but different content.

---

## 🖥️ 15. PuTTY for Windows Users

**PuTTY** is an SSH client for Windows.

Steps:

1️⃣ Convert `.pem` → `.ppk` using **PuTTYgen**.
2️⃣ Open PuTTY:

* Hostname: `ec2-user@<PUBLIC-IP>`
* SSH → Auth → Select `.ppk` file
  3️⃣ Connect.

---

## 🔒 16. Session Manager (SSM) — No SSH Needed

**AWS Systems Manager Session Manager** lets you connect to EC2 **without SSH**, **without key pairs**, and **without open port 22**.

### 🧪 Lab 15 — Connect via Session Manager

1️⃣ Ensure EC2 has **SSM Agent** (most AMIs already do).
2️⃣ Create IAM Role with:

* `AmazonSSMManagedInstanceCore`

3️⃣ Attach IAM Role to EC2.
4️⃣ In EC2 console → **Connect → Session Manager → Start session**.

✅ You get a shell in browser — secure & keyless.

---

## 🖥️ 17. EC2 Serial Console

* Direct **low-level console** access (OS boot, kernel issues)
* Works even if SSH is broken (firewall, misconfigured network)
* For **Nitro** instances only

### Steps

1️⃣ Enable in EC2 settings: **EC2 Serial Console**.
2️⃣ Stop & start instance (if needed).
3️⃣ EC2 → Select instance → Connect → **EC2 Serial Console**.

---

## ⚙️ 18. Burstable vs Fixed Performance Instances

### 🌡️ Burstable Instances

* Example: `t2`, `t3`, `t4g`
* Have **CPU credits**
* When idle → earn credits
* When busy → use credits to “burst” to higher CPU

Great for:

* Low/medium workloads with occasional spikes

### 🧱 Fixed Performance

* Example: `m5`, `c5`, `r5`
* No credit system
* Steady, consistent CPU

Great for:

* Constant, predictable workloads

---

## ⚖️ 19. Load Balancer (ELB) + Lab

**Elastic Load Balancing (ELB)** distributes traffic across multiple EC2 instances.

### Types

* **ALB (Application LB)** → Layer 7 (HTTP/HTTPS), microservices, path-based routing
* **NLB (Network LB)** → Layer 4 (TCP/UDP), ultra-low latency
* **Gateway LB** → For firewalls & 3rd-party appliances
* **Classic LB** → Old, legacy

---

### 🧪 Lab 16 — ALB with 2 EC2 Web Servers

1️⃣ **Launch 2 EC2 instances**

* Install Apache and host different content:

  * EC2-1: `"<h1>Server 1</h1>"`
  * EC2-2: `"<h1>Server 2</h1>"`

2️⃣ **Create Target Group**

* EC2 → **Target Groups**
* Target type: **Instances**
* Add both EC2 instances
* Health check: `/`

3️⃣ **Create Application Load Balancer**

* EC2 → **Load Balancers → Create**
* Type: **Application Load Balancer**
* Scheme: **Internet-facing**
* Listeners: HTTP 80
* Attach Security Group allowing HTTP
* Default action: Forward to **Target Group**

4️⃣ **Test**

* Copy ALB DNS name → open in browser
* Refresh multiple times → traffic alternates between EC2-1 & EC2-2.

---

## 📈 20. Auto Scaling + CloudWatch

**Auto Scaling Group (ASG)** automatically adds/removes EC2 instances based on demand.

Key settings:

* **Min** capacity
* **Max** capacity
* **Desired** capacity

### 🧱 Launch Template (LT)

Reusable config containing:

* AMI
* Instance type
* Security group
* Key pair
* User data
* EBS mappings

---

### 🧪 Lab 17 — Create ASG + Target Tracking Policy

1️⃣ **Create Launch Template**

* EC2 → **Launch Templates → Create**
* Configure AMI, instance type, SG, key, user data if needed.

2️⃣ **Create Auto Scaling Group**

* Use the Launch Template
* Select subnets in at least 2 AZs
* Set:

  * Min: `1`
  * Desired: `1`
  * Max: `3`

3️⃣ **Attach to ALB**

* In ASG wizard → attach to existing Target Group of ALB.

4️⃣ **Add Scaling Policy (Target Tracking)**

* Example: Keep average **CPU at 50%**
* ASG adds more instances when CPU > 50%

5️⃣ **Generate Load (inside EC2)**

```bash
sudo yum install stress -y
stress --cpu 60 --timeout 300
```

* Monitor: CloudWatch → Metrics → ASG/EC2
* ASG should launch new instances when CPU stays high.

---

## 📊 21. CloudWatch Metrics & Alarms

**Amazon CloudWatch**:

* Collects **metrics** (CPU, Network, Disk, etc.)
* Creates **alarms**
* Can trigger **actions** (SNS, ASG, Lambda)

### Alarm States:

* `OK` → normal
* `ALARM` → threshold breached
* `INSUFFICIENT_DATA` → not enough data

---

### 🧪 Lab 18 — Create CPU Alarm for EC2

1️⃣ Enable **detailed monitoring** (optional) when launching EC2.
2️⃣ Go to **CloudWatch → Metrics → EC2 → Per-Instance Metrics**.
3️⃣ Select metric: `CPUUtilization`.
4️⃣ Click “Create alarm”.
5️⃣ Condition:

* Example: **CPU > 70% for 5 minutes**

6️⃣ Action: (optional) send notification using SNS.
7️⃣ Generate load:

```bash
sudo yum install stress -y
stress --cpu 20 --timeout 300
```

8️⃣ Watch alarm change state to **ALARM** in CloudWatch.

---

## 💼 22. EC2 Interview Questions (DevOps Focus)

**Concepts**

1. What is Amazon EC2?
2. Difference between EC2 & Lambda?
3. What is an AMI? Types?
4. What is user data and when does it run?
5. What is the difference between EBS and Instance Store?

**Storage & Backup**

6. What is an EBS snapshot?
7. Explain EBS volume types.
8. How to move data from one instance to another using snapshots?

**Networking & Access**

9. What is an ENI?
10. Difference between Security Group & NACL? (for EC2 view)
11. How to connect EC2 without SSH (no 22 open)?
12. What is Session Manager?

**High Availability & Scaling**

13. Difference between vertical & horizontal scaling?
14. What is an Auto Scaling Group?
15. How do ALB + ASG work together?
16. Explain target tracking scaling policy.

**Cost Optimization**

17. On-Demand vs Reserved vs Spot?
18. When to use Spot instances?
19. When to use Savings Plans instead of RIs?

**Troubleshooting**

20. What if you lost your private key?
21. EC2 not reachable over SSH — how do you troubleshoot?
22. How do you debug boot issues? (Serial Console / User data logs)

---

## 🏁 23. Final Summary (What You Learned)

By now you understand:

✅ What is EC2 & core concepts
✅ How to install Apache / Nginx on Linux & host websites
✅ How to run web apps on Ubuntu & Windows (IIS)
✅ AMIs, snapshots, backups, custom AMIs
✅ EBS vs Instance Store + EBS volume types
✅ How to attach & mount EBS volumes + `/etc/fstab`
✅ EC2 instance families, naming, and purchasing options
✅ SSH keys, recovery tricks, PuTTY, Session Manager
✅ User data & shell scripting automation
✅ ENI, multi-IP, virtual hosting
✅ Load Balancers (ALB) + Target Groups
✅ Auto Scaling Groups + CloudWatch + Scaling Policies
✅ CloudWatch metrics & alarms for EC2
✅ Interview-style questions to revise quickly


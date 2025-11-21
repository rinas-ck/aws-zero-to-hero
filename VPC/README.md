----- PART 1 (COPY EVERYTHING BELOW THIS LINE) -----

<div align="center">

<img src="https://img.shields.io/badge/AWS%20Cloud-VPC%20Networking-black?style=for-the-badge&logo=amazonaws">

<h1 style="color:#00eaff;">🌐 AWS VPC – Zero to Hero (Full Networking Guide)</h1>

<p style="color:#bfbfbf;">
Subnets • Routing • IGW • NAT • Endpoints • Peering • Transit Gateway • Labs Included
</p>

</div>

---

# 🌐 What is a VPC?

A **Virtual Private Cloud (VPC)** is your own isolated network inside AWS.  
You control everything:

- IP ranges (CIDR)
- Public & Private subnets
- Routing
- Internet access
- NAT connections
- Security Groups & NACLs
- VPN, Direct Connect
- Peering
- Transit Gateway
- Endpoints
- Flow logs

---

# 🧠 CIDR (IP Address Range)

Your VPC starts with an IPv4 range called **CIDR Block**.

Example (most common):
```
10.0.0.0/16
```

This gives:
- 65,536 IPs  
- Can be divided into multiple subnets  

---

# 🧱 Subnets (Public & Private)

A **subnet** is a section of your VPC placed inside one specific Availability Zone.

### **🔹 Public Subnet**
- Has route to **Internet Gateway (IGW)**
- Resources can receive public IPs
- Used for:
  - Load balancers
  - Bastion hosts
  - NAT Gateway
  - Public EC2

### **🔹 Private Subnet**
- No direct route to the internet
- Used for:
  - Application servers
  - Databases (RDS)
  - Internal microservices
  - Backend EC2

---

# 🌍 Internet Gateway (IGW)

Needed to allow:

- Public EC2 → Internet (Outbound)
- Internet → Public EC2 (Inbound, if SG allows)

To use IGW:
1. Create IGW
2. Attach to VPC
3. Add route:
```
0.0.0.0/0 → igw-xxxxxxx
```

---

# 🛣 Route Tables

Route tables decide **where traffic goes**.

Every subnet must be associated with exactly **ONE** route table.

### Example Public Route Table:
```
10.0.0.0/16   local
0.0.0.0/0     igw-1234
```

### Example Private Route Table (with NAT):
```
10.0.0.0/16   local
0.0.0.0/0     nat-4567
```

---

# 🔁 NAT Gateway (For Private Subnets)

Private instances cannot access the internet unless you add a **NAT Gateway**.

- Deployed in **public subnet**
- Has **Elastic IP**
- Only **outbound** internet allowed

Private subnet route table example:

```
0.0.0.0/0 → nat-xxxx
```

NAT is used for:
- OS updates
- Package installs (yum, apt)
- Reaching public endpoints safely

NAT does **NOT** allow inbound connections from internet.

-----

END OF PART 1

----- PART 2  -----

# 🛡️ Security Groups (SG)

**Security Groups = Virtual Firewalls for EC2 / ENI / ALB / RDS**

### ⭐ Features:
- **STATEFUL** (if inbound allowed, outbound automatically allowed)
- Apply at **instance level**
- Only **ALLOW** rules (no deny)
- Evaluate **all rules together**

### Example:
Inbound:
- Port 22 (SSH) → My IP
- Port 80 (HTTP) → 0.0.0.0/0

Outbound:
- All traffic allowed (default)

---

# 🚫 Network ACL (NACL)

**Network ACL = Firewall for Subnets**

### ⭐ Features:
- **STATELESS** (need inbound + outbound rules)
- Attached to **subnets**
- Supports **allow + deny**
- Rules processed by **rule number** (lowest first)

### Use Cases:
- Block a malicious IP
- Allow-only specific CIDR ranges

---

# 🔐 SG vs NACL Comparison

| Feature | Security Group | NACL |
|--------|----------------|------|
| Scope | Instance/ENI | Subnet |
| Stateful | ✅ Yes | ❌ No |
| Allows | Allow Only | Allow + Deny |
| Rule Evaluation | All Together | Lowest rule first |
| Use Case | App firewall | Network-level firewall |

---

# 🧵 Elastic Network Interface (ENI)

A **virtual network card** attached to EC2.

### Features:
- Private IPs
- Security groups
- MAC address
- Can move ENI between EC2 instances

### Use Cases:
- High availability
- Multi-homed EC2
- Network appliances

---

# 🌐 VPC Endpoints (Private AWS Access)

VPC Endpoints allow access to AWS services **without internet**.

---

## 🔶 Gateway Endpoint
Used for:
- **S3**
- **DynamoDB**

Added to **route tables**.

Example route table entry:
```
pl-xxxx (gateway endpoint)
```

---

## 🔷 Interface Endpoint (AWS PrivateLink)
Used for:
- SSM
- ECR
- CloudWatch
- KMS
- Secrets Manager
- Lambda
- Many more…

Creates:
- Elastic Network Interface (ENI)
- Private DNS name

### Benefits:
- No NAT needed
- No IGW needed
- More secure (private traffic)

---

# 📜 DHCP Option Sets

Controls how EC2 gets:
- DNS server
- Domain name
- NTP server

Default:
- AmazonProvidedDNS

---

# 🌐 IPv6 in VPC

AWS supports **dual-stack** (IPv4 + IPv6).

### IPv6 Features:
- Globally unique
- No NAT needed
- You can assign /56 to VPC
- Each subnet gets /64

---

# 📄 VPC Flow Logs

Logs network traffic:

- ACCEPT / REJECT
- Source & destination
- Ports
- Packet count
- Bytes count

Flow logs can send data to:
- CloudWatch Logs
- S3

### Uses:
- Debug SG / NACL issues
- Security investigation
- Network visibility

---

# 🧰 Putting It All Together (Architecture Overview)

```
                 +----------------------------+
                 |        PUBLIC SUBNET       |
                 |----------------------------|
Internet <-----> | IGW  | Bastion EC2         |
                 |       NAT Gateway          |
                 +----------------------------+
                          |
                          | Outbound Only
                          v
                 +----------------------------+
                 |        PRIVATE SUBNET      |
                 |----------------------------|
                 | App EC2 | RDS | Internal   |
                 | S3 via VPC Endpoint        |
                 +----------------------------+
```

Your VPC now has:
- Public EC2 (bastion)
- Private EC2 (backend)
- NAT gateway
- S3 endpoint
- Full routing control

-----

END OF PART 2

----- PART 3  -----

# 🧪 LAB 1 — Create a Full Custom VPC (Public + Private Subnets)

🎯 **Goal:**  
Build a full custom VPC with one public and one private subnet.

---

## 🔹 Step 1 — Create VPC

1. Go to **VPC Console → Your VPCs → Create VPC**
2. Choose:
   - **VPC name:** aws-zero-to-hero-vpc
   - **IPv4 CIDR:** `10.0.0.0/16`
3. Create VPC

---

## 🔹 Step 2 — Create Public Subnet

1. **Subnets → Create Subnet**
2. Select VPC: `aws-zero-to-hero-vpc`
3. Name: `public-subnet-a`
4. AZ: `ap-south-1a`
5. CIDR: `10.0.1.0/24`
6. After creation:
   - Select subnet → **Edit subnet settings**
   - Enable **Auto-assign public IPv4**

---

## 🔹 Step 3 — Create Private Subnet

1. **Subnets → Create**
2. Name: `private-subnet-a`
3. CIDR: `10.0.2.0/24`

---

## 🔹 Step 4 — Create & Attach Internet Gateway

1. **Internet Gateways → Create**
2. Name: `igw-zero-to-hero`
3. Select IGW → **Attach to VPC**
4. Choose: `aws-zero-to-hero-vpc`

---

## 🔹 Step 5 — Create Public Route Table

1. **Route Tables → Create Route Table**
   - Name: `public-rt`
2. Routes → Add:
```
0.0.0.0/0 → igw-xxxxxx
```
3. Subnet Associations:
   - Select `public-subnet-a`

🎉 Public subnet ready.

-----

# 🧪 LAB 2 — Add NAT Gateway for Private Subnet

🎯 Goal: Private EC2 gets **outbound internet** safely.

---

## 🔹 Step 1 — Create Elastic IP

1. EC2 → **Elastic IPs**
2. Allocate new Elastic IP

---

## 🔹 Step 2 — Create NAT Gateway

1. **NAT Gateway → Create**
2. Subnet: `public-subnet-a`
3. Attach Elastic IP
4. Wait until status = **Available**

---

## 🔹 Step 3 — Create Private Route Table

1. **Route Tables → Create**
   - Name: `private-rt`
2. Routes → Add:
```
0.0.0.0/0 → nat-xxxxxxxx
```
3. Subnet Association:
   - Select `private-subnet-a`

🎉 Private subnet now has safe internet (outbound only).

-----

# 🧪 LAB 3 — Launch EC2 in Public Subnet (Bastion)

🎯 Goal: Use this EC2 to SSH into private EC2 later.

---

## Steps:

1. **EC2 → Launch instance**
2. Name: `bastion-host`
3. VPC: `aws-zero-to-hero-vpc`
4. Subnet: `public-subnet-a`
5. Auto-assign public IP: **Enable**
6. Security Group:
   - SSH (22) → **Your IP**
7. Launch with your key pair

🎉 Bastion ready.

-----

# 🧪 LAB 4 — Launch EC2 in Private Subnet

🎯 Goal: Launch backend EC2 without public IP.

---

## Steps:

1. **EC2 → Launch instance**
2. Name: `private-app-ec2`
3. Subnet: `private-subnet-a`
4. Auto-assign public IP: **Disable**
5. Security Group: `sg-private-app`
   - SSH (22) → Source = **sg-bastion**
6. Launch

🎉 Private EC2 ready.

-----

# 🧪 LAB 5 — SSH Flow (Laptop → Bastion → Private EC2)

## 🔹 Step 1 — Connect to Bastion EC2

```
ssh -i mykey.pem ec2-user@<Bastion-Public-IP>
```

## 🔹 Step 2 — From Bastion → Private EC2

```
ssh -i mykey.pem ec2-user@<Private-EC2-Private-IP>
```

You are now INSIDE the private subnet 💥

-----

# 🧪 LAB 6 — Configure S3 VPC Endpoint (NO NAT Needed)

🎯 Goal: Private EC2 access S3 **without internet or NAT**.

---

## Steps:

1. **VPC → Endpoints → Create Endpoint**
2. Service: `com.amazonaws.<region>.s3`
3. Type: **Gateway Endpoint**
4. Select:
   - VPC: `aws-zero-to-hero-vpc`
   - Route table: `private-rt`
5. Create

Now test inside private EC2:

```
aws s3 ls
```

It works without internet 🎉

-----

# 🧪 LAB 7 — SG vs NACL Behavior Test

🎯 Goal: Understand stateful vs stateless behavior.

---

## Steps:

1. Edit NACL of public subnet.
2. Add DENY rule:
   - Inbound port 80 → Deny from `0.0.0.0/0`
3. Try opening your web server public IP → **fails**
4. Remove Deny → works again

Conclusion:
- SG is instance-level
- NACL is subnet-level
- NACL DENY overrides SG ALLOW

-----

END OF PART 3

----- PART 4  -----

# 🤝 VPC Peering (Connecting Two VPCs)

A **VPC Peering connection** allows two VPCs to communicate **privately** using their private IP addresses.

### ⭐ What Peering Allows:
- EC2 (VPC A) → EC2 (VPC B)
- Private communication
- Low latency
- Same-region or cross-region

---

# ⭐ When to Use VPC Peering

Use peering when:
- You have 2–3 VPCs
- Simple 1-to-1 connectivity
- Same-company / same-project VPCs
- Need private, low-latency communication

---

# 🛑 VPC Peering Limitations

❌ *No transitive peering*  
Example:
```
VPC A — peered — VPC B — peered — VPC C
```
A **cannot** talk to C.

❌ Cannot reference peered VPC security groups  
❌ No overlapping CIDR blocks allowed  
❌ Cannot use IPv6 from one region to another via peering  

---

# 🧪 LAB 8 — Create VPC Peering Between Two VPCs

### 🎯 Goal:
Connect:
- **VPC-A : 10.0.0.0/16**
- **VPC-B : 10.1.0.0/16**

So EC2 in A can talk to EC2 in B via private IPs.

---

## 🔹 Step 1 — Create Second VPC (VPC-B)

1. **Create VPC**
   - Name: `vpc-b`
   - CIDR: `10.1.0.0/16`

2. Create subnets same as earlier:
   - Public: `10.1.1.0/24`
   - Private: `10.1.2.0/24`

---

## 🔹 Step 2 — Create Peering Connection

1. Go to **VPC → Peering Connections**
2. Create Peering Connection
   - Requester VPC: `aws-zero-to-hero-vpc (A)`
   - Accepter VPC: `vpc-b (B)`

3. Click **Create**

---

## 🔹 Step 3 — Accept Peering Request

1. In the same menu:
2. Select the peering connection
3. Click **Accept Request**

---

## 🔹 Step 4 — Update Route Tables

### In VPC A route table:

Add:
```
Destination: 10.1.0.0/16
Target: pcx-xxxxxx
```

### In VPC B route table:

Add:
```
Destination: 10.0.0.0/16
Target: pcx-xxxxxx
```

---

## 🔹 Step 5 — Test EC2 Communication

Launch EC2 in:
- VPC A: 10.0.1.x
- VPC B: 10.1.1.x

From EC2-A:
```
ping 10.1.1.10
```

If SG allows → ping works.

🎉 **VPC Peering successful!**

---

# 🏛 Transit Gateway (TGW) — Advanced Multi-VPC Networking

Transit Gateway is AWS’s **network hub** that connects:

- Many VPCs
- VPN connections
- Direct Connect
- On-prem networks
- Inter-region traffic (if enabled)

---

# ⭐ Why Use TGW Instead of Peering?

| Feature | Peering | Transit Gateway |
|--------|---------|------------------|
| Best for | 2–3 VPCs | 10+ VPCs |
| Transitive routing | ❌ No | ✅ Yes |
| Complexity | Simple | Enterprise-level |
| Scalability | Limited | Very high |
| Route management | Manual | Central |

---

# 🧠 TGW Core Concepts

### 1️⃣ **Transit Gateway**
The central hub.

### 2️⃣ **Transit Gateway Attachment**
Connection between TGW and:
- VPC
- VPN
- DX
- Another TGW

### 3️⃣ **TGW Route Table**
Controls which attachment can talk to which.

---

# 🧪 LAB 9 — Transit Gateway with Two VPCs

🎯 Goal: Connect **3 VPCs** through TGW:

```
            +----------------+
            |  Transit GW    |
            +----------------+
             /       |        \
         VPC A    VPC B     VPC C
```

All VPCs should communicate via TGW.

---

## 🔹 Step 1 — Create 3 VPCs

### VPC-A:
```
10.0.0.0/16
```

### VPC-B:
```
10.1.0.0/16
```

### VPC-C:
```
10.2.0.0/16
```

Each VPC should have:
- 1 public subnet
- 1 private subnet
- Route tables created (local only for now)

---

## 🔹 Step 2 — Create Transit Gateway

1. Go to **VPC → Transit Gateways**
2. Create TGW
3. Default settings are OK

---

## 🔹 Step 3 — Create Attachments

For each VPC, create attachment:

1. **Transit Gateway Attachments → Create attachment**
2. Type: **VPC**
3. Choose:
   - TGW
   - VPC (A, B, C)
   - Subnets: pick 1 subnet per AZ

Repeat for:
- VPC-A
- VPC-B
- VPC-C

---

## 🔹 Step 4 — Update TGW Route Table

Default route table:
```
10.0.0.0/16 → tgw-attach-A
10.1.0.0/16 → tgw-attach-B
10.2.0.0/16 → tgw-attach-C
```

TGW is now central hub.

---

## 🔹 Step 5 — Update VPC Route Tables

### In VPC-A route table:
```
10.1.0.0/16 → tgw-attach-A
10.2.0.0/16 → tgw-attach-A
```

### In VPC-B route table:
```
10.0.0.0/16 → tgw-attach-B
10.2.0.0/16 → tgw-attach-B
```

### In VPC-C route table:
```
10.0.0.0/16 → tgw-attach-C
10.1.0.0/16 → tgw-attach-C
```

---

## 🔹 Step 6 — Test Communication

Launch EC2 in each VPC:

- EC2-A → 10.0.x.x  
- EC2-B → 10.1.x.x  
- EC2-C → 10.2.x.x  

From EC2-A:
```
ping 10.1.5.10
ping 10.2.3.20
```

Everything should work.

🎉 **TGW fully working. Multi-VPC communication achieved.**

-----

END OF PART 4

----- PART 5  -----

# 📡 VPN & Direct Connect (High-Level Overview)

### 🔹 Site-to-Site VPN
- Connect on-prem data center to AWS VPC.
- Uses **public internet**.
- IPSec encrypted.

### 🔹 Direct Connect
- Dedicated private fiber link.
- Not internet-based.
- High bandwidth (1Gbps–100Gbps).
- Lower latency & more stable.

Use Cases:
- Hybrid cloud
- Large data migrations
- Secure enterprise workloads

---

# 📜 DNS in VPC (Route 53 Resolver)

Every VPC has:
- DNS hostname option
- DNS resolution option

Route 53 Resolver:
- Outbound endpoint (VPC → on-prem DNS)
- Inbound endpoint (on-prem → VPC DNS)

---

# 🧩 Multi-AZ Architecture Best Practices

- Spread subnets across **at least 2 AZs**
- Public Subnets:
  - ALB, NAT Gateways
- Private Subnets:
  - EC2 Apps, RDS
- Highly available NAT:
  - 1 NAT per AZ (best practice)

Architecture sample:

```
          +------------------+
 Internet |       ALB        |
--------->|  Public Subnet A |
          +------------------+
               |        |
               v        v
        Private A   Private B
         App EC2     App EC2
```

---

# 📊 VPC Flow Logs – Deeper Example

Example flow log entry:
```
2 123456789 vpc-xx eni-xx 10.0.1.10 10.1.2.5 443 52054 6 10 840 1617902400 ACCEPT OK
```

Meaning:
- Source IP: 10.0.1.10
- Destination IP: 10.1.2.5
- Ports: 443 & 52054
- ACCEPT → allowed by SG/NACL

---

# 🧩 Common VPC Architectures

### 🔹 1. Public + Private 2-Tier App
- Public: ALB, Bastion, NAT
- Private: EC2 app, RDS

### 🔹 2. VPC Peering Mesh
For 3–4 VPCs max.

### 🔹 3. Transit Gateway Hub
For 10+ VPCs enterprise networks.

### 🔹 4. VPC with S3 Endpoint only (cost-save)
No NAT → access S3 via endpoint only.

---

# 💼 Interview Questions (Real AWS/DevOps Questions)

This section makes your README **interview-ready** 💥

### ⭐ **Core Concepts**
1. What is a VPC?
2. Difference between public and private subnets.
3. What is CIDR?
4. What resources can be in public subnet?
5. Why do private subnets need NAT Gateway?

---

### ⭐ **Routing & Security**
6. What is Internet Gateway?
7. How do Route Tables work?
8. Difference between SG and NACL.
9. What is the difference between stateful and stateless?
10. Why is NACL deny useful?

---

### ⭐ **Advanced**
11. What is VPC endpoint? Types?
12. What is VPC Peering? Why no transitive routing?
13. What is Transit Gateway?
14. When to use TGW instead of peering?
15. How does Bastion → Private EC2 SSH work?

---

### ⭐ **Bonus**
16. What are VPC Flow Logs?
17. What is DNS hostname vs DNS resolution?
18. What is DHCP Option Set?
19. What is Virtual Private Gateway (VGW)?
20. What is Direct Connect?

---

# 🏁 Final Summary (What You Learned)

By now you understand the entire AWS VPC networking flow:

### ✔ IP addressing & CIDR  
### ✔ Public vs Private subnets  
### ✔ Route tables (local, IGW, NAT rules)  
### ✔ Internet Gateway (public routing)  
### ✔ NAT Gateway (private outbound internet)  
### ✔ Security Groups (stateful)  
### ✔ NACLs (stateless, subnet firewall)  
### ✔ VPC Endpoints (private S3/Service access)  
### ✔ ENIs, DHCP, IPv6  
### ✔ Flow Logs (network logging)  
### ✔ VPC Peering (simple VPC-to-VPC)  
### ✔ Transit Gateway (enterprise multi-VPC)  
### ✔ FULL labs:
- Custom VPC
- Public EC2
- Private EC2
- Bastion SSH
- NAT Gateway
- S3 Endpoint
- SG vs NACL
- VPC Peering
- Transit Gateway

Congrats bro — **your VPC README is now better than 90% DevOps candidates**.

-----

END OF PART 5




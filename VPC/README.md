<div align="center">

<kbd> AWS </kbd> <kbd> VPC NOTES </kbd>

</div>

# 🌐 Amazon VPC (Virtual Private Cloud)

Amazon VPC lets you create a **private, isolated network** inside AWS where you launch your resources (EC2, RDS, etc.).

You control:

- ✅ Your IP address range (CIDR)
- ✅ Subnets (public / private)
- ✅ Route tables
- ✅ Internet / NAT gateways
- ✅ Security Groups & Network ACLs
- ✅ Connectivity (VPN, Direct Connect, Peering, Endpoints)

---

## 🧠 Core Concepts

### 🧩 VPC

- **VPC** = Virtual network in AWS.
- Example CIDR: `10.0.0.0/16` → gives you many private IPs.
- A VPC spans **all Availability Zones** in a Region.

---

### 🧱 Subnets

Subnets are **slices of your VPC CIDR** inside specific AZs.

- **Public Subnet**
  - Has a route to the **Internet Gateway (IGW)**.
  - Instances can have **public IPs** and reach internet directly.

- **Private Subnet**
  - **No direct route** to IGW.
  - Uses **NAT Gateway** or **VPC endpoints** to reach internet / AWS services.

**Public vs Private Subnet**

| Type        | Route to 0.0.0.0/0          | Usage                                |
|------------|-----------------------------|--------------------------------------|
| Public     | Internet Gateway (IGW)      | ALB, Bastion, NAT GW, public EC2    |
| Private    | NAT GW / No route / Endpoint| App servers, DB, internal services  |

---

### 📐 CIDR Block

- CIDR = IP range of your network.
- Example:
  - VPC: `10.0.0.0/16`
  - Public Subnet: `10.0.1.0/24`
  - Private Subnet: `10.0.2.0/24`

---

### 🚏 Route Tables

- Define **where traffic goes**.
- Routes are checked using **destination CIDR**.
- Common destinations:
  - `10.0.0.0/16` → `local` (always there, for inside VPC)
  - `0.0.0.0/0` → `igw-xxxx` (internet)
  - `0.0.0.0/0` → `nat-xxxx` (private → internet)
  - `pl-xxxx` / specific CIDR → `vpce-xxxx` (VPC endpoints)

Each subnet is **associated** with exactly **one** route table.

---

### 🌍 Internet Gateway (IGW)

- Horizontally scaled, redundant component that allows:
  - **Inbound** traffic from internet to public resources.
  - **Outbound** internet access from public subnets.
- Must be:
  1. **Created** in the VPC.
  2. **Attached** to that VPC.
  3. Referenced in a **route table** (`0.0.0.0/0 → igw-xxxx`).

---

### 🔁 NAT Gateway

- Managed **Network Address Translation** service.
- Placed in a **public subnet** with an Elastic IP.
- Allows **instances in private subnets** to:
  - Access internet **outbound** (updates, package installs).
  - Block unsolicited **inbound** traffic from internet.

Route example for private subnet route table:

- `0.0.0.0/0` → `nat-xxxxxxxx`

---

### 🛡️ Security Groups vs NACLs

**Security Group (SG)**

- **Stateful** (response traffic is automatically allowed).
- Attached to **ENI / EC2 / RDS**.
- Rules are **allow-only** (no explicit deny).
- Common usage: Allow SSH / HTTP / HTTPS to instance.

**Network ACL (NACL)**

- **Stateless** (need both inbound + outbound rules).
- Attached to **subnets**.
- Rules can be **allow or deny**.
- Evaluated in **rule number order** (lowest first).

| Feature             | Security Group          | NACL                         |
|--------------------|-------------------------|------------------------------|
| Scope              | ENI / Instance          | Subnet                       |
| Stateful?          | ✅ Yes                  | ❌ No                        |
| Allow / Deny       | Allow only              | Allow + Deny                |
| Typical Use        | App-level firewall      | Subnet-level firewall       |

---

### 🔗 VPC Endpoints

Access AWS services **privately** without using public internet.

- **Gateway Endpoint**
  - Target = Route table entry.
  - Used for: **S3**, **DynamoDB**.
- **Interface Endpoint**
  - Elastic Network Interface (ENI) with private IP.
  - Used for: many AWS services (SSM, CloudWatch, etc.).

---

### 🌉 VPC Peering & Transit Gateway (High Level)

- **VPC Peering**
  - Connects **two VPCs** (same or different accounts / regions).
  - Non-transitive (A–B and B–C does **not** mean A–C).
- **Transit Gateway (TGW)**
  - Central hub to connect **many VPCs + VPNs**.
  - Scales better than many peering connections.

---

## 🗺️ Typical 2-Tier VPC Design (Interview Friendly)

Example design:

- VPC: `10.0.0.0/16`
- Public Subnets:
  - `10.0.1.0/24` (AZ-a)
  - `10.0.3.0/24` (AZ-b)
- Private Subnets:
  - `10.0.2.0/24` (AZ-a)
  - `10.0.4.0/24` (AZ-b)
- Internet Gateway attached.
- NAT Gateways in each public subnet.
- Route tables:
  - Public: `0.0.0.0/0 → IGW`
  - Private: `0.0.0.0/0 → NAT GW`
- Security Groups:
  - ALB SG: Allow HTTP/HTTPS from internet.
  - App SG: Allow HTTP from ALB SG only.
  - DB SG: Allow DB port from App SG only.

This architecture is **great to explain in interviews**.

---

## 🔧 LAB 1 – Create a Basic VPC (Console)

🎯 Goal: Create custom VPC with one public and one private subnet.

1. **Create VPC**
   - Go to **VPC Console → Your VPCs → Create VPC**.
   - Name: `aws-zero-to-hero-vpc`
   - IPv4 CIDR: `10.0.0.0/16`
   - Tenancy: Default
   - Create VPC.

2. **Create Public Subnet**
   - **Subnets → Create subnet**
   - VPC: `aws-zero-to-hero-vpc`
   - Name: `public-subnet-a`
   - AZ: choose `ap-south-1a` (example)
   - CIDR: `10.0.1.0/24`
   - After create → **Edit subnet settings** → enable  
     `Auto-assign public IPv4 address`.

3. **Create Private Subnet**
   - Name: `private-subnet-a`
   - AZ: same or different (e.g. `ap-south-1a`)
   - CIDR: `10.0.2.0/24`

---

## 🔧 LAB 2 – Internet Access (IGW + NAT)

🎯 Goal: Give **internet access** to public & private subnets correctly.

### 2.1 Attach Internet Gateway

1. **Internet Gateways → Create internet gateway**
   - Name: `aws-zero-to-hero-igw`
2. Select IGW → **Actions → Attach to VPC**  
   - Choose `aws-zero-to-hero-vpc`.

### 2.2 Public Route Table

1. **Route tables → Create route table**
   - Name: `public-rt`
   - VPC: `aws-zero-to-hero-vpc`.
2. Select `public-rt` → **Routes → Edit routes**
   - Add: `Destination: 0.0.0.0/0 → Target: Internet Gateway (igw-xxxx)`
3. **Subnet associations**
   - Associate `public-subnet-a` with `public-rt`.

Now any instance in `public-subnet-a` with a **public IP** can reach internet.

---

### 2.3 NAT Gateway for Private Subnet

1. Create **Elastic IP** (EC2 → Network & Security → Elastic IPs).
2. **NAT Gateways → Create NAT Gateway**
   - Subnet: `public-subnet-a`
   - Elastic IP: the one you created.
3. Wait until NAT GW becomes **Available**.
4. **Route tables → Create route table**
   - Name: `private-rt`
   - VPC: `aws-zero-to-hero-vpc`.
5. **Routes → Edit routes** on `private-rt`
   - `Destination: 0.0.0.0/0 → Target: nat-xxxxxxxx`.
6. **Subnet associations**
   - Associate `private-subnet-a` with `private-rt`.

✅ Now:
- Public subnet → IGW.
- Private subnet → NAT → internet (outbound only).

---

## 🔧 LAB 3 – Security Groups vs NACL

🎯 Goal: Understand traffic control at **instance** vs **subnet** level.

### 3.1 Security Group

1. Go to **EC2 → Security Groups → Create security group**
   - Name: `web-sg`
   - VPC: `aws-zero-to-hero-vpc`
2. Inbound rules:
   - HTTP (80) → Source: `0.0.0.0/0`
   - SSH (22) → Source: your IP.
3. Launch an EC2 instance in `public-subnet-a` with `web-sg`.
4. Test:
   - `ping` from your machine.
   - `curl` the instance public IP (after installing a web server).

### 3.2 NACL

1. **Network ACLs → Create network ACL**
   - Name: `public-nacl`
   - VPC: `aws-zero-to-hero-vpc`.
2. Inbound rules:
   - 100: Allow HTTP (80) from `0.0.0.0/0`
   - 110: Allow ephemeral ports `1024-65535` from `0.0.0.0/0`
3. Outbound rules:
   - 100: Allow `0.0.0.0/0` (all traffic).
4. **Subnet associations**
   - Associate `public-subnet-a` with `public-nacl`.

📝 Try adding a **Deny** rule for your own IP and see how it blocks you even if SG allows it. Great learning!

---

## 🔧 LAB 4 – S3 Gateway VPC Endpoint

🎯 Goal: Access S3 from private subnet **without NAT / internet**.

1. (Optional) Stop using NAT (remove `0.0.0.0/0 → NAT` from `private-rt`).
2. **Endpoints → Create endpoint**
   - Service category: AWS services.
   - Service: `com.amazonaws.<region>.s3`
   - Type: **Gateway**
   - VPC: `aws-zero-to-hero-vpc`
   - Route tables: select `private-rt`.
3. Launch a small EC2 in `private-subnet-a`.
4. From that instance:
   - Use AWS CLI:  
     `aws s3 ls s3://your-bucket-name`
5. It works through the **VPC endpoint**, no internet.

---

## 🔧 LAB 5 – VPC Peering (Bonus)

🎯 Goal: Connect two VPCs so they can talk using **private IPs**.

1. Create another VPC:
   - `10.1.0.0/16` with one subnet.
2. **VPC Peering → Create peering connection**
   - Requester: `aws-zero-to-hero-vpc`
   - Accepter: `10.1.0.0/16` VPC (same account).
3. **Accept** peering request.
4. Update **route tables**:
   - In `aws-zero-to-hero-vpc` route table → add `10.1.0.0/16 → pcx-xxxx`
   - In second VPC route table → add `10.0.0.0/16 → pcx-xxxx`
5. Launch EC2 instances in each VPC and ping using **private IPs**.

---

## 📝 Quick Interview Points (VPC)

- VPC is a **logically isolated network** inside AWS.
- Public subnet = route to **IGW**, private subnet = route to **NAT / no internet**.
- **Security Groups** are stateful, instance-level; **NACLs** are stateless, subnet-level.
- NAT Gateway gives **outbound internet** to private subnets.
- VPC Endpoints let you access AWS services **without public internet**.
- VPC Peering is **non-transitive**; Transit Gateway centralizes many connections.

---

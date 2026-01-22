# 🚀 AWS VPC Monitoring & Connectivity (Flow Logs + CloudWatch Logs Insights)

This project is a hands-on implementation of **AWS VPC networking + monitoring** where I built multiple VPCs, configured peering, tested connectivity using EC2, and analyzed real network traffic using **VPC Flow Logs** and **CloudWatch Logs Insights**.

Without a VPC, all AWS resources would exist in one giant open cloud network — like a country with no cities or districts.  
A **VPC (Virtual Private Cloud)** gives you an isolated network where you can define:

✅ IP ranges  
✅ Subnets (Public/Private)  
✅ Security rules  
✅ Connectivity between resources  
✅ Monitoring and traffic visibility  

---

## 📌 Project Goals

✅ Build and configure **two separate VPCs**  
✅ Create **subnets, route tables, Internet Gateway (IGW), Security Groups, and NACLs**  
✅ Establish **VPC Peering** between VPCs  
✅ Launch EC2 instances and validate connectivity  
✅ Enable **VPC Flow Logs → CloudWatch Log Group**  
✅ Run **CloudWatch Logs Insights queries** to analyze traffic patterns  

---

## 🧠 Networking Concepts (Explained Simply)

### ✅ What is a VPC?
A **VPC** is your private network environment inside AWS.  
It’s the reason you can launch resources that are private to you and control how they communicate without using the public internet.

---

### ✅ What is a CIDR Block?
**CIDR (Classless Inter-Domain Routing)** defines an IP address range for your VPC or subnet.

#### Examples

| CIDR Block | IP Address Range |
|-----------|------------------|
| `10.0.0.0/8`  | `10.0.0.0` → `10.255.255.255` |
| `10.0.0.0/16` | `10.0.0.0` → `10.0.255.255` |
| `10.0.0.0/24` | `10.0.0.0` → `10.0.0.255` |

---

### ✅ What are Subnets?
If a VPC is a city, **subnets are neighborhoods** inside that city.

- **Public Subnet 🌐** → connected to the internet  
- **Private Subnet 🔐** → no direct internet access  

📌 **Important rule:** Subnets inside the same VPC must **not overlap** (each subnet must have a unique CIDR block).

---

### ✅ What is an Internet Gateway (IGW)?
An **Internet Gateway** connects your VPC to the internet.

It allows resources (like EC2 in a public subnet) to:
- access the internet (outbound)
- be accessed from the internet (inbound)

---

### ✅ What is a Route Table?
A **Route Table** works like a GPS for subnet traffic.

It contains routes (rules) that define where traffic should go.  
Every subnet must be associated with a route table.

---

### ✅ What does `0.0.0.0/0` mean?
`0.0.0.0/0` represents **all IPv4 addresses**.

If a subnet route table contains:
- destination: `0.0.0.0/0`
- target: **Internet Gateway**

➡️ That subnet becomes a **public subnet**.

---

### ✅ What is a Security Group (SG)?
A **Security Group** is like a security guard for a specific resource (like an EC2 instance).

✅ Security Groups attach to **resources**, not subnets  
✅ They control:
- inbound traffic (who can enter)
- outbound traffic (what your instance can send)

They filter traffic using:
- IP addresses  
- protocols (HTTP, SSH, FTP, SMTP, etc.)  
- port numbers  

---

### ✅ What is a Network ACL (NACL)?
A **Network ACL** acts like traffic police for an entire subnet.

✅ NACLs apply to **subnets**, not individual instances  
✅ Useful for broad rules like:
- block a range of IPs  
- deny traffic on specific ports  

---

### ✅ Security Group vs NACL (Quick Difference)

| Feature | Security Group | Network ACL |
|--------|-----------------|-------------|
| Applies to | Instance/Resource | Subnet |
| Level | Fine-grained | Broad |
| Works like | Security guard at building | Traffic police at neighborhood |
| Controls | Inbound + Outbound | Inbound + Outbound |

---

## 🖥️ EC2 Basics Used in This Project

### ✅ Key Pair
Key pairs enable secure access to EC2 instances using cryptography.

- Public key → stored on EC2  
- Private key → stored with the user  

Only someone with the private key can authenticate successfully.

---

### ✅ What is an AMI?
**AMI (Amazon Machine Image)** is a template used to create EC2 instances.  
It includes:
- OS  
- required base software  
- configuration setup  

---

### ✅ Instance Type
If AMI = software blueprint, instance type = hardware configuration.

It defines:
- CPU  
- RAM  
- performance level  
- storage/network capacity  

---

### ✅ SSH (Secure Shell)
SSH is the secure protocol used to connect to EC2 instances.

Once connected, all communication is encrypted, making it safe for remote access.

---

## 🌐 NAT Gateway vs Internet Gateway

### ✅ NAT Gateway
NAT Gateway allows **private subnet instances** to access the internet (**outbound only**).

✅ Useful for:
- installing updates  
- downloading patches  
- accessing external APIs securely  

❌ Does **NOT** allow inbound connections from the internet.

---

### ✅ Internet Gateway
Internet Gateway allows **public subnet instances** to communicate with the internet both ways.

✅ inbound + outbound

---

## 🔁 VPC Connectivity + Testing

### ✅ EC2 Instance Connect
Instead of using local SSH terminal, **EC2 Instance Connect** allows secure browser-based access from AWS Console.

---

### ✅ ICMP (Ping) Traffic
Ping uses **ICMP (Internet Control Message Protocol)** to test connectivity.

ICMP is often blocked by default to prevent abuse (example: ping flood attacks).  
Allowing **All ICMP - IPv4** helps troubleshoot connectivity issues.

---

### ✅ Real Ping Output Concepts
While pinging between servers, I observed:

- **time** → latency (round trip time)  
- **TTL (Time to Live)** → packet lifespan across routers  
- **sequence number** → matches requests & replies  

---

## 🔗 VPC Peering Connection

A **VPC peering connection** is a private connection between two VPCs.

✅ Traffic flows between VPCs using **private IP addresses**  
✅ Communication stays inside AWS network (no public internet)

Without peering, VPC-to-VPC communication would require public routing.

---

## 📌 Elastic IP (EIP)

An Elastic IP is a **static public IPv4 address**.

Why it matters:
- EC2 public IPs are usually dynamic (change after stop/start)  
- Elastic IP provides a permanent public address  

---

## 📊 Monitoring with Amazon CloudWatch

### ✅ CloudWatch Overview
AWS services publish metrics and logs to CloudWatch.

CloudWatch helps you:
- visualize performance  
- monitor health  
- troubleshoot issues  
- build dashboards & alarms  

---

### ✅ Log Group
A **Log Group** is like a folder where related logs are stored together.

📌 Logs are region-specific, but dashboards can combine data across regions.

---

### ✅ Logs
Logs are the history/diary of what happens inside your system:
- access attempts  
- errors  
- system events  
- traffic details  

---

### ✅ VPC Flow Logs
Flow logs capture traffic information for a VPC (or subnet / network interface).

They record:
- source IP  
- destination IP  
- ports  
- accept/reject actions  
- bytes transferred  

---

### ✅ CloudWatch Logs Insights
Logs Insights allows you to query logs using filters and analytics.

Useful for:
✅ troubleshooting  
✅ identifying rejected traffic  
✅ understanding who is talking to whom  
✅ traffic pattern analysis  

---

### ✅ What is a Network Interface (ENI)?
An **Elastic Network Interface** is automatically attached to your EC2 instance and connects it to your VPC.

It acts as the networking layer that enables sending and receiving data.

---

## 🛠️ Tech Stack / AWS Services Used

- AWS VPC  
- Subnets (Public)  
- Internet Gateway  
- Route Tables  
- Security Groups  
- Network ACLs  
- VPC Peering  
- EC2 Instances + EC2 Instance Connect  
- VPC Flow Logs  
- CloudWatch Log Groups  
- CloudWatch Logs Insights  

---

## ✅ Key Learnings

🔹 Learned real-world AWS networking by building the setup manually  
🔹 Understood the difference between **Security Group vs NACL** in actual traffic behavior  
🔹 Verified connectivity using private and public IP addressing  
🔹 Gained hands-on experience with **VPC Flow Logs**  
🔹 Used Logs Insights queries to observe:
- ACCEPT vs REJECT traffic  
- bytes transferred  
- top IP talkers  

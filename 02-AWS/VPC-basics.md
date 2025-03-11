# 🌐 Amazon VPC (Virtual Private Cloud) - Beginner Guide

Amazon **VPC (Virtual Private Cloud)** allows you to create a **custom network** within AWS, where you can launch AWS resources securely. It provides **full control over networking**, including **IP addressing, subnets, routing, and security settings**.

---

## 🏗️ Steps to Create a VPC in AWS

### 1️⃣ **Log in to AWS Console**
- Navigate to the **AWS Management Console**.
- Search for **VPC** in the **AWS Services** search bar.
- Click on **VPC Dashboard**.

### 2️⃣ **Create a VPC**
- Click on **Create VPC**.
- Choose **VPC only** or **VPC and more** (if you want subnets, IGW, etc.).
- Enter the **Name tag** (e.g., `MyFirstVPC`).
- Choose **IPv4 CIDR Block** (e.g., `10.0.0.0/16` for 65,536 IPs).
- Optionally, enable **IPv6 CIDR Block**.
- Click **Create VPC**.

### 3️⃣ **Create Subnets**
- Go to **Subnets** → **Create Subnet**.
- Select your **VPC**.
- Choose an **Availability Zone** (e.g., `us-east-1a`).
- Enter a **Subnet Name** (e.g., `Public-Subnet`).
- Assign an **IPv4 CIDR Block** (e.g., `10.0.1.0/24`).
- Click **Create Subnet**.
- **Repeat the process** for Private Subnets.

### 4️⃣ **Create an Internet Gateway (IGW)**
- Go to **Internet Gateways** → **Create Internet Gateway**.
- Enter **Name tag** (e.g., `MyInternetGateway`).
- Click **Create Internet Gateway**.
- **Attach IGW to your VPC**:
  - Select the IGW → Click **Actions** → **Attach to VPC**.
  - Choose your **VPC** → Click **Attach**.

### 5️⃣ **Configure Route Tables**
- Go to **Route Tables** → **Create Route Table**.
- Name it **Public-RT** → Select your **VPC** → Click **Create**.
- **Add Routes for Internet Access**:
  - Select the **Public-RT** → **Edit Routes**.
  - Add a new route:
    - Destination: `0.0.0.0/0`
    - Target: **Your Internet Gateway**
  - Click **Save Changes**.
- **Associate Public Subnet**:
  - Select the **Public-RT** → Click **Edit Subnet Associations**.
  - Choose your **Public Subnet** → Click **Save**.

### 6️⃣ **Create Security Groups**
- Go to **Security Groups** → **Create Security Group**.
- Enter **Name tag** (e.g., `Web-SG`).
- Select your **VPC**.
- Add **Inbound Rules**:
  - **For Public Access (Web Server)**:
    - Protocol: **HTTP**
    - Port: **80**
    - Source: `0.0.0.0/0` (for public access)
  - **For SSH (Secure Access)**:
    - Protocol: **SSH**
    - Port: **22**
    - Source: **Your IP Address** (for security)
- Click **Create Security Group**.

---

## 🚀 Testing Your VPC
- Launch an **EC2 Instance** in the **Public Subnet**.
- Attach the **Public Security Group**.
- Assign an **Elastic IP** if needed.
- Try to connect via **SSH** or open the **web server**.

---

## 🛡️ Best Practices for VPC Security
- 🔒 **Use Private Subnets** for databases or sensitive resources.
- 🔑 **Enable Network ACLs** for additional security.
- 🚀 **Use NAT Gateway** for private subnet internet access.
- 📝 **Monitor network activity** using **VPC Flow Logs**.

---

## 📚 Further Learning
- 📖 **[AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/)**
- 📺 **AWS VPC Explained - YouTube**

🔗 **Mastering VPC is key to setting up secure and scalable AWS environments!** 🌍🔥

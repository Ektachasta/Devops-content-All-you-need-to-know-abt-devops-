Welcome to the **AWS EC2 Guide**! 🚀

# 📖 AWS EC2 (Elastic Compute Cloud)

## 🔹 What is EC2?  
**AWS EC2 (Elastic Compute Cloud)** is a **virtual server in the cloud** that allows you to run applications on demand.  

It provides **scalable, resizable computing capacity**, making it easy to deploy and manage servers without investing in physical hardware.  

## 🔹 Why Use EC2?  
✅ **On-demand virtual machines** for hosting applications, databases, or backend services.  
✅ **Scalability** – Increase or decrease computing resources as needed.  
✅ **Pay-as-you-go pricing** – Only pay for the resources you use.  
✅ **Different instance types** optimized for CPU, memory, or storage.  
✅ **Integrates with AWS services** like S3, RDS, and VPC.  

---

## 🛠️ **How to Launch an EC2 Instance (Step-by-Step Guide)**  

1️⃣ **Go to AWS Console → EC2 Dashboard**  
   - Click **"Launch Instance"**.  

2️⃣ **Choose an Amazon Machine Image (AMI)**  
   - Select an operating system like **Amazon Linux, Ubuntu, or Windows**.  
   - Example: Choose **Amazon Linux 2023** for a simple setup.  

3️⃣ **Choose an Instance Type**  
   - Instance types define CPU, memory, and network performance.  
   - **t2.micro (Free Tier eligible)** – Good for basic web servers.  
   - **m5.large** – Optimized for high-performance computing.  

4️⃣ **Configure Instance Details**  
   - Number of instances: `1` (For now, start with one instance).  
   - Network: Choose **default VPC** (or create a custom VPC).  
   - Subnet: Select an **available public subnet**.  
   - Enable **Auto-assign Public IP** to allow external access.  

5️⃣ **Add Storage**  
   - Default **8GB EBS volume** (Elastic Block Storage).  
   - Increase if needed, e.g., **30GB** for larger applications.  

6️⃣ **Configure Security Group (Firewall Settings)**  
   - Security groups control incoming/outgoing traffic.  
   - Allow **SSH (port 22)** for remote access.  
   - Allow **HTTP (port 80) & HTTPS (port 443)** for web traffic.  

7️⃣ **Create or Select a Key Pair (For SSH Access)**  
   - Choose **"Create a new key pair"**.  
   - Download the `.pem` file – **store it securely**!  

8️⃣ **Launch the Instance!** 🎉  
   - Click **Launch Instance**, and AWS will set up your EC2 server.  

---

## 🖥️ **Connecting to Your EC2 Instance (SSH Login)**  

1️⃣ Open **Terminal or Command Prompt** (Mac/Linux/Windows).  
2️⃣ Navigate to the directory where your **.pem key file** is saved.  
3️⃣ Run the SSH command to connect:  

```sh
ssh -i my-key.pem ec2-user@<EC2-Public-IP>
```

- Replace `my-key.pem` with your **key pair file**.  
- Replace `<EC2-Public-IP>` with your **instance’s public IP** (found in the AWS console).  

4️⃣ If successful, you will see the **EC2 Linux shell** 🎉.  

---

## 🚀 **Deploying a Simple Web Server on EC2**  

1️⃣ **Update the System**  

```sh
sudo yum update -y
```

2️⃣ **Install Apache (Web Server)**  

```sh
sudo yum install httpd -y
```

3️⃣ **Start Apache & Enable it on Boot**  

```sh
sudo systemctl start httpd
sudo systemctl enable httpd
```

4️⃣ **Create a Simple Webpage**  

```sh
echo "<h1>Welcome to My AWS EC2 Web Server!</h1>" | sudo tee /var/www/html/index.html
```

5️⃣ **Allow HTTP Traffic in Security Group**  
   - Go to **AWS EC2 Console → Security Groups**.  
   - Edit inbound rules: **Allow HTTP (port 80) for all (0.0.0.0/0)**.  

6️⃣ **Visit Your Website**  
   - Open a browser and go to:  
     ```
     http://<EC2-Public-IP>
     ```
   - You should see **"Welcome to My AWS EC2 Web Server!"** 🎉  

---

## 🔹 **Types of EC2 Instances**  

AWS provides different **EC2 instance families** optimized for specific workloads:  

| **Instance Type** | **Use Case** |
|------------------|-------------|
| **t2.micro** (Free Tier) | Basic web servers, low-traffic websites |
| **m5.large** | General-purpose applications, databases |
| **c5.xlarge** | Compute-intensive applications (Machine Learning, Data Processing) |
| **r5.large** | Memory-intensive applications (Caching, In-Memory Databases) |
| **i3.large** | High-performance storage workloads (Big Data, Analytics) |

---

## 🚀 Summary  

- **AWS EC2** is a **powerful cloud computing service** that lets you run applications on virtual machines.  
- You can **launch an EC2 instance**, connect via SSH, and deploy a **web server** in minutes.  
- EC2 offers **different instance types** for various workloads and **flexible pricing options**.  
- Secure your instance using **VPC, Security Groups, and IAM Roles**.


# ☁️ Amazon S3 (Simple Storage Service)

Amazon S3 (Simple Storage Service) is a cloud storage service provided by AWS that allows users to store and retrieve any amount of data at any time from anywhere.

## 📌 Why Use Amazon S3?
- ✅ **Scalable** – Stores unlimited data.
- ✅ **Durable & Secure** – Provides 99.999999999% (11 9's) durability.
- ✅ **Cost-Effective** – Pay only for what you use.
- ✅ **High Availability** – Your data is always accessible.
- ✅ **Flexible Storage Classes** – Choose different storage classes based on your needs.

---

## 🏗️ How Does S3 Work?
Amazon S3 stores data in **buckets**. A bucket is like a **folder** where you store your files (objects).

- Each object consists of:
  - **Data** – The actual file.
  - **Metadata** – Information about the file.
  - **Key** – The unique identifier for each object.

---

## 🛠️ Getting Started with S3

### 1️⃣ **Create an S3 Bucket**
1. Sign in to your **AWS Console**.
2. Navigate to **S3 Service**.
3. Click on **"Create bucket"**.
4. Choose a **unique bucket name** (must be globally unique).
5. Select a **region** (where your data will be stored).
6. Configure settings like:
   - **Public or Private Access**
   - **Versioning** (keeps multiple versions of files)
   - **Encryption** (for security)
7. Click **Create**!

### 2️⃣ **Upload Files to S3**
1. Open your **S3 bucket**.
2. Click **Upload**.
3. Select the file you want to upload.
4. Set permissions (public or private).
5. Click **Upload**.

### 3️⃣ **Accessing an S3 Object**
Every object in S3 has a **unique URL** like:
**https://<bucket-name>.s3.<region>.amazonaws.com/<file-name>**

⚠️ **Note:** If the file is private, you will need AWS credentials to access it.

---

## 🏷️ S3 Storage Classes
Amazon S3 offers **different storage classes** based on cost, performance, and access frequency:

| Storage Class | Use Case | Cost |
|--------------|---------|------|
| **S3 Standard** | Frequently accessed data | High |
| **S3 Intelligent-Tiering** | Automatically moves data between storage classes | Medium |
| **S3 Standard-IA** | Infrequently accessed data | Lower |
| **S3 One Zone-IA** | Infrequent access, single AZ | Lower |
| **S3 Glacier** | Long-term archival storage | Very Low |
| **S3 Glacier Deep Archive** | Lowest-cost storage, for long-term backups | Lowest |

---

## 🔒 Securing Your S3 Data
- **Bucket Policies** – Define who can access your data.
- **IAM Roles** – Control access permissions at the user level.
- **Encryption** – Protect your data using:
  - Server-Side Encryption (SSE-S3, SSE-KMS, SSE-C)
  - Client-Side Encryption
- **MFA Delete** – Adds extra security for deletion.

---

## 🌍 Hosting a Static Website with S3
Amazon S3 can also be used to **host static websites** (HTML, CSS, JS).

1. Enable **Static Website Hosting** in S3 settings.
2. Upload your website files (index.html, styles.css, etc.).
3. Configure **Bucket Policy** to allow public access.
4. Your website is now accessible via the S3 endpoint URL!

---

## 🏆 S3 Best Practices
- ✅ Use **S3 Lifecycle Policies** to move old data to cheaper storage classes.
- ✅ Enable **Versioning** to protect against accidental deletions.
- ✅ Set up **S3 Replication** to copy data across AWS regions.
- ✅ Monitor usage with **AWS CloudWatch & S3 Access Logs**.

---

## 🎯 Conclusion
Amazon S3 is an essential AWS service that provides **secure, scalable, and cost-effective** cloud storage. Whether you need storage for backups, hosting a website, or large-scale data analytics, S3 has a solution for you!

🚀 **Start using S3 today and manage your cloud storage efficiently!** 🚀


# 🔐 AWS IAM (Identity and Access Management)

AWS IAM (Identity and Access Management) is a service that helps you securely **control access** to AWS resources. It allows you to manage **who** can access AWS services and **what** actions they can perform.

---

## 📌 Why Use AWS IAM?
- ✅ **Secure Access** – Controls who can access AWS resources.
- ✅ **Granular Permissions** – Define fine-tuned access rules.
- ✅ **Multi-Factor Authentication (MFA)** – Adds extra security.
- ✅ **Temporary Access** – Uses roles for short-term permissions.
- ✅ **Logging & Auditing** – Tracks all activity via AWS CloudTrail.

---

## 🏗️ Key Components of IAM

AWS IAM consists of **Users, Groups, Policies, and Roles**.

### 1️⃣ **Users** 👤
- Represents **a single person** or application.
- Each user has:
  - A **username**
  - **Access credentials** (passwords, access keys)
  - Assigned **permissions** (what they can do)

### 2️⃣ **Groups** 👥
- A **collection of users** with similar permissions.
- Example: A "Developers" group can have permissions for EC2 and S3.

### 3️⃣ **Policies** 📜
- A **set of rules** that define what actions are allowed/denied.
- Written in **JSON format**.
- Example policy that allows reading from an S3 bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}

---
## 🔄 Roles 
- A **temporary identity** for AWS services or external users.
- Used by **applications or AWS services** instead of users.
- **Example**: An **EC2 instance** can assume a role to access an **S3 bucket**.

---

## 🛠️ Getting Started with IAM

### 🏗️ Creating an IAM User
1. Go to **AWS IAM Console**.
2. Click **"Users" → "Add user"**.
3. Enter a **username**.
4. Select **AWS Management Console access** or **Programmatic access**.
5. Assign **permissions** (attach policies or add user to a group).
6. Click **"Create User"**.

---

### 🔑 Creating an IAM Policy
1. Go to **IAM Console** → **Policies** → **Create Policy**.
2. Select **JSON** and define permissions

---
## 🔄 Creating an IAM Role
1. Go to **IAM Console** → **Roles** → **Create Role**.
2. Choose a **trusted entity** (AWS Service, Another AWS Account, etc.).
3. Attach **permissions** (e.g., S3 Full Access).
4. Name the role and **create** it.
5. Assign the role to an **EC2 instance** or any AWS service.

---

## 🛡️ Best Practices for IAM Security
- 🔐 **Enable MFA (Multi-Factor Authentication)** for extra security.
- 🚫 **Follow the Principle of Least Privilege** (grant only necessary permissions).
- 🔍 **Regularly review IAM users & permissions**.
- 🔒 **Use IAM Roles** instead of long-term access keys.
- 📝 **Monitor IAM activity** using **AWS CloudTrail**.

---

## 📚 Further Learning
- 📖 **[AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/)**
- 📺 **AWS IAM Basics - YouTube**

🚀 **IAM is the foundation of AWS security. Mastering it ensures better cloud security and efficient access management!** 🔥









 

# 📜 AWS CLI Cheat Sheet

This is a quick reference guide for commonly used AWS CLI (Command Line Interface) commands for managing various AWS services like EC2, S3, Lambda, IAM, RDS, VPC, and more.

## 📖 Table of Contents
- [Volumes](#volumes)
- [AMIs](#amis)
- [Lambda](#lambda)
- [IAM](#iam)
- [S3 API](#s3-api)
- [VPC](#vpc)
- [Subnets](#subnets)
- [Internet Gateway](#internet-gateway)
- [NAT](#nat)
- [Route Tables](#route-tables)
- [CloudFront](#cloudfront)
- [RDS](#rds)

## 💾 Volumes
### ✅ Describe Volumes
```bash
aws ec2 describe-volumes
```
### ✅ Describe Volumes Using Profile
```bash
aws ec2 describe-volumes --profile <profile_name>
```
### ✅ List Available Volume IDs
```bash
aws ec2 describe-volumes --filters Name=status,Values=available
```
### ✅ Delete a Volume
```bash
aws ec2 delete-volume --volume-id <volume_id>
```
### ✅ Create a Snapshot
```bash
aws ec2 create-snapshot --volume-id <volume_id>
```

## 🖼 AMIs (Amazon Machine Images)
### ✅ List AMIs
```bash
aws ec2 describe-images
```
### ✅ Describe an AMI
```bash
aws ec2 describe-images --image-ids <image_id>
```
### ✅ List Amazon AMIs
```bash
aws ec2 describe-images --owners amazon
```

## ⚙️ Lambda (Serverless Functions)
### ✅ List Functions
```bash
aws lambda list-functions
```
### ✅ Invoke a Function
```bash
aws lambda invoke --function-name <function_name> response.json
```
### ✅ Update a Function Code
```bash
aws lambda update-function-code --function-name <function_name> --zip-file fileb://function.zip
```
### ✅ Publish a Version
```bash
aws lambda publish-version --function-name <function_name>
```

## 🔐 IAM (Identity and Access Management)
### ✅ List Users
```bash
aws iam list-users
```
### ✅ List Policies
```bash
aws iam list-policies
```
### ✅ List Groups
```bash
aws iam list-groups
```
### ✅ Get Users in a Group
```bash
aws iam get-group --group-name <group_name>
```
### ✅ List Access Keys
```bash
aws iam list-access-keys
```

## 🗄 S3 API (Simple Storage Service)
### ✅ List Buckets
```bash
aws s3api list-buckets
```
### ✅ List Only Bucket Names
```bash
aws s3api list-buckets --query 'Buckets[].Name'
```
### ✅ Sync Local Folder with S3 Bucket
```bash
aws s3 sync <local_folder> s3://<bucket_name>
```
### ✅ Copy Files to S3
```bash
aws s3 cp <file_name> s3://<bucket_name>
```
### ✅ Delete a File from S3
```bash
aws s3 rm s3://<bucket_name>/<file_name>
```
### ✅ Empty a Bucket
```bash
aws s3 rm s3://<bucket_name> --recursive
```
### ✅ Delete a Bucket
```bash
aws s3 rb s3://<bucket_name> --force
```

## 💻 VPC (Virtual Private Cloud)
### ✅ Create a VPC
```bash
aws ec2 create-vpc --cidr-block <cidr_block>
```
### ✅ Modify DNS Hostnames
```bash
aws ec2 modify-vpc-attribute --vpc-id <vpc_id> --enable-dns-hostnames "{\"Value\":true}"
```

## 🏠 Subnets
### ✅ Create a Subnet
```bash
aws ec2 create-subnet --vpc-id <vpc_id> --cidr-block <cidr_block>
```
### ✅ Assign Public IP Automatically
```bash
aws ec2 modify-subnet-attribute --subnet-id <subnet_id> --map-public-ip-on-launch
```

## 🌐 Internet Gateway
### ✅ Create an Internet Gateway
```bash
aws ec2 create-internet-gateway
```
### ✅ Attach IGW to VPC
```bash
aws ec2 attach-internet-gateway --internet-gateway-id <igw_id> --vpc-id <vpc_id>
```

## 🌉 NAT (Network Address Translation)
### ✅ Create a NAT Gateway
```bash
aws ec2 create-nat-gateway --subnet-id <subnet_id> --allocation-id <allocation_id>
```

## 🚦 Route Tables
### ✅ Create a Public Route Table
```bash
aws ec2 create-route-table --vpc-id <vpc_id>
```
### ✅ Associate a Route Table to Subnet
```bash
aws ec2 associate-route-table --route-table-id <route_table_id> --subnet-id <subnet_id>
```

## 📡 CloudFront (Content Delivery Network)
### ✅ List Distributions
```bash
aws cloudfront list-distributions
```
### ✅ Create an Invalidation
```bash
aws cloudfront create-invalidation --distribution-id <distribution_id> --paths '/*'
```
### ✅ Sync and Invalidate Files
```bash
aws s3 sync . s3://<bucket_name> --acl public-read && \
aws cloudfront create-invalidation --distribution-id <distribution_id> --paths '/*'
```

## 💽 RDS (Relational Database Service)
### ✅ List Databases
```bash
aws rds describe-db-instances
```
### ✅ Create a DB Instance
```bash
aws rds create-db-instance \
 --db-instance-identifier <db_identifier> \
 --db-instance-class db.t3.micro \
 --engine mysql \
 --master-username <username> \
 --master-user-password <password> \
 --allocated-storage 20
```
### ✅ Describe Backups
```bash
aws rds describe-db-instance-automated-backups
```
### ✅ Create a DB Security Group
```bash
aws rds create-db-security-group \
 --db-security-group-name <group_name> \
 --db-security-group-description "My Group"
```
### ✅ Apply Tags to a DB
```bash
aws rds add-tags-to-resource \
 --resource-name arn:aws:rds:<region>:<account_id>:db:<db_name> \
 --tags "[{\"Key\": \"Environment\",\"Value\": \"Production\"}]"
```

## ✅ Bonus Commands
### 🚀 Check AWS CLI Version
```bash
aws --version
```
### 🚀 Configure AWS CLI
```bash
aws configure
```
### 🚀 Set Profile Configuration
```bash
aws configure --profile <profile_name>
```
### 🚀 List All Profiles
```bash
aws configure list-profiles
```

## 💯 Why Use This Cheat Sheet?
- 📜 All commonly used AWS CLI commands are covered.
- ✅ Easy to copy-paste directly into your terminal.
- 🚀 Fast reference without Googling commands.

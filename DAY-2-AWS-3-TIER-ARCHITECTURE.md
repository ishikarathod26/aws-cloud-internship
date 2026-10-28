# Day 2 – AWS 3-Tier Architecture

## Project Overview
Today, I continued learning and implementing the foundational components of a 3-tier architecture on Amazon Web Services (AWS). The goal is to separate the application into public-facing, application, and database layers while following basic security practices.

## AWS Services and Components Practiced

### 1. Amazon VPC
- Created a custom VPC named `three-tier-vpc`.
- Configured the VPC CIDR block as `10.0.0.0/16`.
- Learned how a VPC provides an isolated virtual network for AWS resources.

### 2. Subnets
Created subnets across two Availability Zones in the Mumbai region (`ap-south-1`).

- **Public Subnets:** Intended for internet-facing resources such as an Application Load Balancer.
- **Private Application Subnets:** Intended to host application servers.
- **Private Database Subnets:** Intended to host database resources.

### 3. Internet Gateway and Route Table
- Attached an Internet Gateway to the custom VPC.
- Created a public route table with a default route (`0.0.0.0/0`) pointing to the Internet Gateway.
- Associated the public route table with both public subnets.
- Kept the private subnets on a route table without a direct internet route.

### 4. Security Groups
Created separate security groups to control traffic between the tiers.

- **ALB Security Group:** Allows inbound HTTP traffic on port `80`.
- **Application Security Group:** Allows inbound traffic on port `8080` from the ALB security group.
- **Database Security Group:** Allows inbound MySQL traffic on port `3306` from the application security group.

### 5. Amazon EC2 and IAM
- Practiced launching an Amazon Linux 2023 EC2 instance in a private application subnet.
- Used the `t3.micro` instance type for the practice session.
- Created an IAM role named `three-tier-ec2-ssm-role`.
- Attached the AWS managed policy `AmazonSSMManagedInstanceCore` to the role.
- Verified that the EC2 instance passed its status checks.
- Attempted to connect through AWS Systems Manager Session Manager, but the instance could not register successfully because valid Systems Manager credentials were unavailable. Private network connectivity also needs further investigation.
- Terminated the temporary EC2 instance after the practice session to reduce ongoing compute costs.

## Security Practices Learned
- Keep application servers in private subnets when public access is unnecessary.
- Restrict inbound traffic using security groups.
- Allow database access only from the application tier.
- Use IAM roles instead of storing AWS credentials directly on an EC2 instance.
- Never commit private keys, passwords, access keys, or other secrets to GitHub.

## Pending Tasks
- Verify that the terminated EC2 instance's EBS volume has been deleted.
- Investigate Systems Manager connectivity and the required private network configuration.
- Configure an Application Load Balancer.
- Create and configure Amazon RDS in the private database subnets.
- Deploy an application and test communication between the tiers.
- Review AWS resources and billing to avoid unnecessary costs.

## Current Status
The initial VPC, subnet, routing, security group, and IAM configuration has been practiced. The complete three-tier architecture is not yet operational. Load balancer configuration, database deployment, application deployment, and end-to-end testing remain pending.

## Key Learning
A three-tier architecture separates public access, application processing, and database storage into distinct layers. Subnets, route tables, security groups, and IAM roles help control network access and permissions between these layers.

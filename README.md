# AWS Cloud Internship

This repository documents my hands-on AWS learning, internship tasks, and practical cloud infrastructure work.

## Day 1 — AWS VPC and Networking Fundamentals

**Date:** 27 October 2026

### Work Completed
- Reviewed existing AWS IAM resources and account security settings.
- Created a custom VPC named `three-tier-vpc`.
- Configured the IPv4 CIDR block as `10.0.0.0/16`.
- Created five subnets across two Availability Zones in the Mumbai region (`ap-south-1`):
  - Two public-designated subnets
  - Two private application-designated subnets
  - One private database-designated subnet

### Concepts Learned
- Virtual Private Cloud (VPC)
- Subnets and CIDR notation
- Public and private subnet concepts
- Availability Zones
- Security Groups
- Internet Gateways
- Route Tables

### Current Status
The VPC and five subnets have been created. One database subnet and the routing configuration are still pending. EC2, RDS, and Application Load Balancer setup have not yet been completed.

### Next Steps
- Create the remaining database subnet.
- Configure routing and network security.
- Set up EC2, RDS, and the Application Load Balancer.
- Test connectivity and document the results.

**Note:** Subnet names indicate their intended purpose; actual public or private connectivity depends on routing and other network settings.

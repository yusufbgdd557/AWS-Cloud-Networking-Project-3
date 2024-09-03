
# AWS Cloud Networking: Deploying EC2 Instances in Public and Private Subnets within a VPC

## Overview

In this project, I set up an Amazon VPC with both public and private subnets, configured security settings such as Security Groups and Network ACLs, and launched EC2 instances in each subnet. Below is a step-by-step breakdown of what I accomplished.

## VPC and Public Subnet Configuration

1. **Created VPC**  
   CIDR Block: `10.0.0.0/16`

2. **Created Public Subnet**  
   - CIDR Block: `10.0.0.0/24`
   - Availability Zone: `us-east-1a`
   - Enabled auto-assign public IPv4.

3. **Created Internet Gateway**  
   - Attached to the VPC to allow the public subnet to connect to the internet.

4. **Created Public Route Table**  
   - **Destination:** `0.0.0.0/0`  
   - **Target:** Internet Gateway (for internet access).
   - Associated My Public Subnet with this route table.

5. **Created Public Security Group**  
   - Allowed inbound traffic on HTTP (`80`) from Anywhere-IPv4 (`0.0.0.0/0`).
   - No need to specify outbound rules, as Security Groups are stateful (outbound traffic is automatically allowed if inbound traffic is permitted).

6. **Created Network ACL for Public Subnet**  
   - Allowed all inbound and outbound traffic (for demonstration purposes).
   - Associated the Network ACL with the Public Subnet.

## VPC and Private Subnet Configuration

7. **Created Private Subnet**  
   - CIDR Block: `10.0.1.0/24` (Avoiding overlap with public subnet)
   - Availability Zone: `us-east-1b` (For redundancy and high availability)
   - No internet access (hence no need for an Internet Gateway).

8. **Created Private Route Table**  
   - **Destination:** `local` (automatically set to `10.0.0.0/16` to allow internal routing within the VPC).  
   - **Target:** `local` (handled by AWS internally for VPC traffic).
   - Left the route table with the default settings since AWS handles internal routing automatically for communication between subnets within the same VPC.
   - Associated My Private Subnet with this route table.

9. **Created Private Network ACL**  
   - Denied both inbound and outbound traffic to maintain the security of the private resources.
   - Associated the Network ACL with the Private Subnet.

## EC2 Instances Launch Configuration
I chose my custom VPC for deploying the EC2 instances instead of using the default VPC provided by AWS.

### Public EC2 Instance ("My Public Server")

10. **Network Configuration**  
    - Subnet: My Public Subnet (I chose it from the drop down menu that shows the existing subnets within My VPC)
    - Security Group: Associated the existing "Public Security Group" which allows HTTP traffic from anywhere.

11. **Launched EC2**  
    - Instance launched with public IP and accessible over the internet.

### Private EC2 Instance ("My Private Server")

12. **Network Configuration**  
    - Subnet: My Private Subnet (I chose it from the drop down menu that shows the existing subnets within My VPC)
    - Security Group: Created a new "Private Security Group."
    - Inbound Rule: Configured a rule with a **custom source**. Allowed inbound SSH traffic (`22`) only from resources that use the "Public Security Group."

13. **Launched EC2**  
    - Instance launched into My Private Subnet, only accessible via SSH from instances associated with the "Public Security Group."

## Conclusion

This project demonstrates how to set up and configure a secure AWS environment with both public and private subnets. The public subnet allows internet access, while the private subnet is isolated. Access between the subnets is controlled via Security Groups, and additional network-level security is enforced using Network ACLs.

For further details on the previous steps, such as creating the VPC, Subnets, Security Groups, and NACLs, check out my earlier repositories:
- [AWS VPC Project](https://github.com/yusufbgdd557/AWS-VPC-Project)
- [AWS VPC Project 2](https://github.com/yusufbgdd557/AWS-VPC-Project-2)

## Project Screenshots

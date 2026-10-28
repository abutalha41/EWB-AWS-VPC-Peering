AWS VPC Peering Project

 Mohammed Abu Talha  
Region: Asia Pacific (Hyderabad) — `ap-south-2`  
project: EWB AWS Networking — VPC Peering

This project connects two Amazon VPCs using a VPC Peering connection. App-A and App-B run in private subnets and communicate through their private IPv4 addresses. A bastion host in VPC A provides the SSH entry point for administration.

The work covers VPC and subnet setup, route tables, security groups, EC2 instances, VPC Peering, SSH access and connectivity testing.


![AWS VPC Peering network layout](architecture.png)
Resource and IP summary

 Resource | Network / private IP | Role 
 `ewb-vpc-a` | `10.10.0.0/16` | VPC A |
 `ewb-a-public` | `10.10.1.0/24` | Public subnet for bastion 
 `ewb-a-private` | `10.10.2.0/24` | Private subnet for App-A 
 `ewb-vpc-b` | `10.20.0.0/16` | VPC B 
 `ewb-b-private` | `10.20.1.0/24` | Private subnet for App-B 
 Bastion | `10.10.1.71` | SSH entry point 
 App-A | `10.10.2.26` | Private instance in VPC A 
 App-B | `10.20.1.184` | Private instance in VPC B 


Main configuration

- VPC A: `10.10.0.0/16`
- VPC B: `10.20.0.0/16`
- Public subnet in VPC A: `10.10.1.0/24`
- Private subnet in VPC A: `10.10.2.0/24`
- Private subnet in VPC B: `10.20.1.0/24`
- Internet Gateway attached to VPC A for bastion access
- VPC Peering between VPC A and VPC B
- Routes to the peer VPC CIDR in both private route tables
- A route to VPC B in the public route table so the bastion can reach App-B through SSH ProxyJump
- SSH and ICMP inbound rules scoped to the required source addresses
Test results

The terminal evidence includes the following:

| Test | Result shown in evidence |
|---|---|
| Baseline ping from App-A to App-B | Failed before the peer path was working |
| SSH access to App-B | Successful login to Amazon Linux 2023 |
| App-A to App-B ping | 4 packets received, 0% packet loss |
| App-B to App-A ping | Initial attempt failed; a later attempt received 4 packets with 0% packet loss |


 What I learned

This project helped me practise subnet planning, route-table configuration, VPC Peering, security-group rules and SSH access to private EC2 instances. It also showed why both sides need a route to the other VPC and why a successful peering status alone does not guarantee that traffic will pass.

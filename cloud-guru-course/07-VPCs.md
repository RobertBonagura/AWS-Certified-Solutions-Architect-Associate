# Amazon VPC

## Introduction to VPC
Amazon VPC lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.

You have complete control over your virtual networking environment, including creating your own IP address range, creation of subnets, and configuration of route tables and network gateways.

You can easily customize the network configuration for your Amazon Virtual Private Cloud.
* For example, you can create a public-facing subnet for your webservers that has access to the Internet,
and place your backend systems such as databases or application servers in a private-facing subnet with no Internet access
* You can leverage multiple levels of security, including security groups and network access control lists to, to help control access to Amazon EC2 instances in each subnet.

Additionally, you can create a Hardware Virtual Private Network (VPN) connection between your corporate datacenter and your VPC and leverage the AWS cloud as an extension of your corporate datacenter.

### What can we do with VPC?
* Launch instances into a subnet of your choosing
* Assign custom IP address ranges in each subnet
* Configure route tables between subnets
* Create internet gateways and attach them to our VPC
* Much better security control over your AWS resources
* Assign instance security groups
* Subnet network access control lists (ACLS)

### Default VPC vs Custom VPC
Default VPC is user friendly, allowing you to immediately deploy instances.

All subnets in default VPC ave a route to the internet.

Each EC2 instance has both a public and private IP address

### VPC Peering
Allows you to connect one VPC with another via a direct network route using private IP addresses.

Instances behave as if they were in the same private network.

You can peer VPC's with other AWS as well as within the same account.

Peering is in a star configuration, which means there is 1 essential VPC that peers with 4 others, there is no Transitive Peering.
* Transitive Peering is when VPC A communicates with another VPC C through VPC B

### Exam Tips
Think of VPC as a logical datacenter in AWS

It consists of IGWs (or Virtual Private Gateways), Route Tables, Network Access Control Lists, Subnets, and Security Groups

No more than 1 Availability Zone per Subnet

Security Groups, are Stateful; Network Access Control Lists are Stateless

There is no Transitive Peering

## Build a Custom VPC Lab - Exam Tips
When you create a VPC a default Route Table, Network Access Control List (NACL) and a default Security Group is created.
* It will not create any subnets, nor will it create a default Internet Gateway.

US-East-1a in your AWS account can be a completely different availability zone to US-East-1a in another AWS account. These are randomized.

Amazon always reserves 5 IP addresses within your subnets

You can only have 1 Internet Gateway per VPC.

Security Groups can't span VPCs.

## NAT Instances & NAT Gateways
Network Address Translation

In order EC2 instances in private subnets to still be able to download software, they need a way of communicating with the Internet Gateway without being made public.
* NAT Instances and NAT Gateways are the solution to this

NAT Instances are an EC2 Instance, and our on their way out. NAT Gateways are more popular now, and are a highly available gateway that allows you to have your private subnets communicate to the internet without being public.


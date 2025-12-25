<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Creating a Private Subnet

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-private)

**Author:** Arun Srinivasan  
**Email:** run2780@gmail.com

---

## Creating a Private Subnet

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-private_afe1fdbd)

---

## Introducing Today's Project!

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.

### How I used Amazon VPC in this project

We created a private subnet that should not be directly accessible from the internet.
Defined a CIDR block (e.g., 10.0.2.0/24) inside the VPC.
Ensured no route to the Internet Gateway was added.
Placed it in a specific Availability Zone for redundancy.
This isolated environment kept sensitive workloads secure while still allowing communication with other subnets inside the VPC.
Private Route table: To control traffic flow for the private subnet.
Created a new route table and associated it with the private subnet.
Added a local route (10.0.0.0/16) for intra-VPC communication.
Configured a route to a NAT Gateway in a public subnet so instances could download updates or connect outward without being exposed.
Resources in the private subnet could reach the internet indirectly (via NAT) but remained unreachable from outside.
Created Private NACL to add an extra layer of security at the subnet level and associated it with the private subnet. 
Configured inbound and outbound traffic rules



### One thing I didn't expect in this project was...

One thing I didn’t expect in this project was how much fine‑grained control the private route table gave us once we introduced the NAT Gateway. At first, it seemed straightforward—just block direct internet access—but the moment we needed selective outbound connectivity (for patching servers or pulling container images), the complexity jumped.

### This project took me...

This project took me around 30 min including documentation

---

## Private vs Public Subnets

The difference between public and private subnets is that public subnets are acceptable by and can access the internet while private subnets are completelty isolated from internet by default.


Having private subnets are useful because keeping resources away from internet is very important for the security of cofidential resources..

My private and public subnets cannot have the same IPV4 CIDR block i.e. same range of IP addressess. The CIDR block for every subnet must be unique and cannot overlap with another subnet

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-private_afe1fdbd)

---

## A dedicated route table

By default, my private subnet is associated with the default route table.i.e the route table that has a route to an internet gateway.

I had to set up a new route table because my subnet cannot have a route to an internet gateway

My private subnet's dedicated route table only has one inbound and one outbound rule that allows internal communication. i.e, with a destination of another resource within my VPC.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-private_b4b904b5)

---

## A new network ACL

By default, my private subnet is associated with a default network ACL set up for every VPC. This default network ACL is associated with your private subnet, since you haven't set up an explicit association between your private subnet and another network ACL.

A VPC's default network ACL allows all traffic, which exposes your private subnet to unrestricted access from the internet or other untrusted networks. 
I set up a dedicated network ACL for my private subnet that restricts traffic and protects your private subnet!

My new network ACL has two simple rules denying all inbound traffic and denying all outbound traffic!

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-private_1ed2cb07)

---

---


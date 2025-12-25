# Creating a Private Subnet

**Author:** Arun Srinivasan | December 2025  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325

---

## Creating a Private Subnet

---

## Project Overview
We've learnt about subnets and even created a public subnet that's connected to the internet.
But what about resources that we want to keep private?
For example, it's very common for engineers to set up an EC2 instance hosting their web app in a public subnet, but keep the database of customers and login details in a private subnet.
This project is to set up your very first private subnet and learn its differences from a public subnet along the way.

#### Get ready to:
* Create a private Subnet
* Create a dedicated(private) route table
* Create a dedicated(private) Network ACL (Network Access Control List).

### Personal reflection
Working through this project gave me a deeper appreciation for how network design decisions directly impact both security and usability. At first, the tasks seemed technical and straightforward—create a subnet, attach a route table, configure a NACL—but as I progressed, I realized each step carried broader implications for how resources interact inside the cloud.
* Learning Curve: I didn’t expect the amount of detail required in configuring routes and ACLs. Even small misconfigurations (like forgetting to associate the subnet with the correct route table) could completely block traffic. This taught me patience and the importance of double‑checking each step.
* Security Awareness: Setting up the private subnet and NACL reinforced the principle of least privilege. It was eye‑opening to see how isolating resources can drastically reduce exposure to external threats while still allowing controlled outbound access through NAT.
* Growth: Completing the project boosted my confidence in cloud networking. I now feel more capable of designing secure architectures and troubleshooting connectivity issues in AWS.
* Problem-Solving: One challenge was balancing connectivity needs with security. For example, ensuring instances could fetch updates without opening them up to the internet. This required creative thinking and a clear understanding of AWS networking components.
  Connectivity needs could be Servers (EC2 instances) in a private subnet often need to reach the internet for legitimate reasons—like downloading operating system updates, pulling Docker images, or connecting to external APIs.
  At the same time, those servers should not be directly exposed to the internet, because that would make them vulnerable to attacks.
  So the “problem‑solving” challenge was figuring out how to let those instances talk outbound safely without allowing inbound internet traffic.

### Key Takeaway
The biggest lesson was that networking in the cloud is not just about connecting resources—it’s about designing trust boundaries. Every subnet, route, and ACL is a deliberate choice that shapes how secure and functional the environment will be.

The entire exercise, including documentation, was completed in approximately 30 minutes, reflecting both my grasp of the theoretical concepts and my ability to apply them effectively in practice. This project reinforced my confidence in working with AWS networking components and highlighted the importance of structured planning when building cloud infrastructure.

One thing I didn’t expect in this project was how much fine‑grained control the private route table gave us once we introduced the NAT Gateway. At first, it seemed straightforward—just block direct internet access—but the moment we needed selective outbound connectivity (for patching servers or pulling container images), the complexity jumped.


### Mission to accomplish:

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/game%20plan.png?raw=true)


### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix withothers.
  
### How I used Amazon VPC in this project

#### Private Subnet
Purpose:
* We created a private subnet to host backend resources (like databases or internal services) that should not be directly accessible from the internet.

Implementation:
* Defined a CIDR block (e.g., 10.0.1.0/24) inside the VPC.
* Ensured no route to the Internet Gateway was added.
* Placed it in a specific Availability Zone for redundancy.
  
Outcome:
* This isolated environment kept sensitive workloads secure while still allowing communication with other subnets inside the VPC.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/create_private_subnet.png?raw=true)

#### Private Route Table
Purpose:
* To control traffic flow for the private subnet.
Implementation:
* Created a new route table and associated it with the private subnet.
* Added a local route (10.0.0.0/16) for intra-VPC communication.
* Configured a route to a NAT Gateway in a public subnet so instances could download updates or connect outward without being exposed.
Outcome:
* Resources in the private subnet could reach the internet indirectly (via NAT) but remained unreachable from outside.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/Private%20route%20table.png?raw=true)

##### A dedicated route table

By default, my private subnet is associated with the default route table.i.e the route table that has a route to an internet gateway.
I had to set up a new route table because my subnet cannot have a route to an internet gateway
My private subnet's dedicated route table only has one inbound and one outbound rule that allows internal communication. i.e, with a destination of another resource within my VPC.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-private_b4b904b5)


#### Private Network ACL (NACL)
Purpose:
* To add an extra layer of security at the subnet level.
Implementation:
* Created a custom NACL and associated it with the private subnet.
* Configured inbound rules to allow only trusted traffic (e.g., from application servers or VPN).
* Configured outbound rules to allow traffic to internal services and NAT Gateway.
* Denied all other traffic by default.
Outcome:
*This hardened the subnet against unauthorized access attempts, complementing security groups at the instance level.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/private_NACL_inbound.png?raw=true)

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/private_NACL_outbound.png?raw=true)

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/private_subnet_association_NACL.png?raw=true)

##### A new network ACL

By default, my private subnet is associated with a default network ACL set up for every VPC. This default network ACL is associated with your private subnet, since you haven't set up an explicit association between your private subnet and another network ACL.

A VPC's default network ACL allows all traffic, which exposes your private subnet to unrestricted access from the internet or other untrusted networks. 
I set up a dedicated network ACL for my private subnet that restricts traffic and protects your private subnet!

My new network ACL has two simple rules denying all inbound traffic and denying all outbound traffic!

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-private_1ed2cb07)

---

## Private vs Public Subnets

The difference between public and private subnets is that public subnets are acceptable by and can access the internet while private subnets are completelty isolated from internet by default.
Having private subnets are useful because keeping resources away from internet is very important for the security of cofidential resources.

My private and public subnets cannot have the same IPV4 CIDR block i.e. same range of IP addressess. The CIDR block for every subnet must be unique and cannot overlap with another subnet.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Creating%20a%20Private%20Subnet/Error_create_subnet.png?raw=true)


---

---


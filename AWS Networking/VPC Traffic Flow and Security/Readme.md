---

## VPC Traffic Flow and Security


---

** Author:Arun Srinivasan  
** Linkedin: www.linkedin.com/in/arun-srinivasan-2a244325

## Project Overview
In VPC Traffic Flow and Security let's keep diving into the core essentials of building an Amazon Virtual Private Cloud (VPC).
#### Get ready to:
* Create a route table.
* Create a security group.
* Create a Network ACL (Network Access Control List).

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix withothers.

### How I used Amazon VPC in this project?

To ensure secure and efficient communication within the cloud environment, I leverage Amazon Virtual Private Cloud (VPC) as the foundation of my project’s networking setup. The focus is on controlling traffic flow and enforcing security at multiple layers.

#### Core Essentials Implemented
##### Route Table
* Defines how traffic is directed within the VPC.
* Associates subnets with specific routes to control communication between internal resources and external networks.
* Ensures proper connectivity to the internet (via Internet Gateway) or private networks (via VPN/Direct Connect).

##### Security Group
* Acts as a virtual firewall at the instance level.
* Controls inbound and outbound traffic for EC2 instances.
* Configured with rules to allow only necessary protocols and ports (e.g., SSH, HTTP/HTTPS).
* Provides stateful filtering, meaning return traffic is automatically allowed.

##### Network ACL (Access Control List)
* Provides an additional layer of security at the subnet level.
* Controls inbound and outbound traffic using stateless rules (explicitly allow or deny).
* Useful for setting broader restrictions across multiple instances in a subnet.
* Helps mitigate risks by blocking unwanted IP ranges or protocols.

### Personal reflection
Completing this project provided me with valuable hands-on experience in the foundational aspects of AWS networking.
This project showed me that cloud infrastructure is not just about deploying resources—it’s about designing secure, scalable systems with intention. I now feel more prepared to approach larger, more complex architectures with confidence. 

The entire exercise, including documentation, was completed in approximately 45 minutes, reflecting both my grasp of the theoretical concepts and my ability to apply them effectively in practice. This project reinforced my confidence in working with AWS networking components and highlighted the importance of structured planning when building cloud infrastructure.

One thing I didn't expect in this project was default network ACL available which is designed to allow all traffic to move freely until you decide to customize the rules to fit your needs.

### Mission to accomplish:

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/VPC%20Traffic%20Flow%20and%20Security/game%20plan.png?raw=true)

---
## Route tables

Route tables are like GPS for the resources in your subnet. Just like a GPS helps people get to their destination in a city, a route table is a table of rules, called routes, that decide where the data in your network should go.
Every subnet in your VPC needs to be linked to a route table, because the table tells your subnet's traffic where to travel to send and receive data. For example, if you have a web server (i.e. an EC2 instance) hosting a website, the EC2 instance's subnet needs a route table that knows how to direct incoming traffic to the website.

Routes tables are needed to make a subnet public because Subnet needs to have a route to an nternet gateway in order to be considered public.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/VPC%20Traffic%20Flow%20and%20Security/Route%20table.png?raw=true)

### Route destination and target

A route table is made up of routes, which are defined by its destination and target.

* Destination: The IP address range that traffic wants to reach.
* Target: The road or path that the traffic will have to take to get to its destination.

- igw-xxxxxx: Means the traffic is routed to the internet via the Internet Gateway.
- local: Means the traffic stays within the VPC, allowing internal communication between resources.

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0 and a target of MyWork IG(internet gateway).

---

## Security groups

If VPCs are cities and subnets are neighbourhoods, a security group is a security checkpoint, or security guard, at the entrance for each building (resource) in that neighbourhood (subnet).
Every resource must be associated with a security group. This means security groups don't attach to a VPC or a subnet, they attach to a specific resource within that VPC/subnet. If you don't specify a security group when you launch a resource, it will use the default.
Security groups are responsible for checking who comes in and out. They have strict rules about what kind of traffic can enter or leave the resource based on its IP address, protocols and port numbers.

Protocols: With VPCs as our city and every resource as a building, think of protocols as different vehicles, like buses, taxis and trucks, to deliver data in different ways. Protocols are special rules that help data move across the internet, each designed to send data for a specific kind of task. 

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-security_92b0b0b4)

### Inbound vs Outbound rules

* Inbound rules: Inbound rules control the data that can enter the resources in your security group. 

In this scenario, setting up inbound rules is important for allowing users to access your public website, while 
Examples of inbound data: Visitors and form submissions to your website.
I  configured an inbound rule that allows all inbound HTTP traffic.

* Outbound rules: Outbound rules are rules to control that data that your resources can send out.
By default, Outbound rule will allow all outbound traffic.
outbound rules help manage how your server interacts with other parts of the internet.
Examples of outbound data: Your server requests data from another service; your app sends out an email notification.

---

## Network ACLs

Think of Network ACLs as traffic cops stationed at every entry and exit point of your subnet, checking each data packet against a table of ACL rules before allowing them through.

### Security groups vs. network ACLs

* Network ACLs are used to set broad traffic rules that apply to an entire subnet. For example, blocking incoming traffic from a particular range of IP addresses or denying all outbound traffic to certain ports.
* Security groups allow for more granular control, managing access to individual resource. You can specify which ports and protocols are allowed for each connected resource.  
<br>
Having both is a great security practice! You can set broad restrictions at the subnet level with ACLs, and more specific limits at the resource level through security groups. This dual layer takes security to the next level as traffic must pass through multiple checks, which reduces the chances of unwanted access.

### Default vs Custom Network ACLs

Similar to security groups, network ACLs use inbound and outbound rules
AWS sets up a default network ACL for every VPC in your account. This default is designed to allow all traffic to move freely until you decide to customize the rules to fit your needs.
Just like security groups, network ACLs use inbound and outbound rules to decide which data packets are allowed to enter or leave subnets:

* Rule 100 Inbound allows all inbound traffic into the Public Subnet.
* Rule 100 Outbound allows all traffic out of the Public Subnet.

The second line in each ruleset shows an asterisk (*) that acts as a catch-all rule in case traffic does not match any of the earlier rules. In our case, since Rule 100 already allows all traffic, the asterisk rule won't actually come into play.
This means default network ACLs allow all inbound and outbound traffic, unless customized.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/VPC%20Traffic%20Flow%20and%20Security/inbound%20rules.png?raw=true)

---


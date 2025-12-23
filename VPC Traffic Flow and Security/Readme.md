<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Traffic Flow and Security

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-security)

**Author:** Arun Srinivasan  
**Email:** run2780@gmail.com

---

## VPC Traffic Flow and Security

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-security_92b0b0b4)

---

## Introducing Today's Project!

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In thisspace, you can set up and run AWS resources in a network that you control. Themain idea is that it’s separate and secure, so your resources don’t mix withothers.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to demonstrate how data flow from client/user to VPC via Security groups and Network ACL.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was default network ACL available which is designed to allow all traffic to move freely until you decide to customize the rules to fit your needs.

### This project took me...

This project took me around 45 minutes including documentation

---

## Route tables

Route tables are like GPS for the resources in your subnet. Just like a GPS helps people get to their destination in a city, a route table is a table of rules, called routes, that decide where the data in your network should go.
Every subnet in your VPC needs to be linked to a route table, because the table tells your subnet's traffic where to travel to send and receive data. For example, if you have a web server (i.e. an EC2 instance) hosting a website, the EC2 instance's subnet needs a route table that knows how to direct incoming traffic to the website.

Routes tables are needed to make a subnet public because Subnet needs to have a route to an nternet gateway in order to be considered public.


![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-security_0a07b191)

---

## Route destination and target

A route table is made up of routes, which are defined by its destination and target.

Destination: The IP address range that traffic wants to reach.
Target: The road or path that the traffic will have to take to get to its destination.

igw-xxxxxx: Means the traffic is routed to the internet via the Internet Gateway.
local: Means the traffic stays within the VPC, allowing internal communication between resources.

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0 and a target of MyWork IG(internet gateway).

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-security_0a07b191)

---

## Security groups

If VPCs are cities and subnets are neighbourhoods, a security group is a security checkpoint, or security guard, at the entrance for each building (resource) in that neighbourhood (subnet).

Every resource must be associated with a security group. This means security groups don't attach to a VPC or a subnet, they attach to a specific resource within that VPC/subnet. If you don't specify a security group when you launch a resource, it will use the default.

Security groups are responsible for checking who comes in and out. They have strict rules about what kind of traffic can enter or leave the resource based on its IP address, protocols and port numbers.

rotocols: With VPCs as our city and every resource as a building, think of protocols as different vehicles, like buses, taxis and trucks, to deliver data in different ways. Protocols are special rules that help data move across the internet, each designed to send data for a specific kind of task. 


### Inbound vs Outbound rules

Inbound rules control the data that can enter the resources in your security group. 

In this scenario, setting up inbound rules is important for allowing users to access your public website, while 
Examples of inbound data: Visitors and form submissions to your website.
I  configured an inbound rule that allows all inbound HTTP traffic.

Outbound rules are rules to control that data that your resources can send out.
By default, Outbound rule will allow all outbound traffic.
outbound rules help manage how your server interacts with other parts of the internet.
Examples of outbound data: Your server requests data from another service; your app sends out an email notification.


![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-security_92b0b0b4)

---

## Network ACLs

Think of Network ACLs as traffic cops stationed at every entry and exit point of your subnet, checking each data packet against a table of ACL rules before allowing them through.

### Security groups vs. network ACLs

Network ACLs are used to set broad traffic rules that apply to an entire subnet. For example, blocking incoming traffic from a particular range of IP addresses or denying all outbound traffic to certain ports.

Security groups allow for more granular control, managing access to individual resource. You can specify which ports and protocols are allowed for each connected resource.

Having both is a great security practice! You can set broad restrictions at the subnet level with ACLs, and more specific limits at the resource level through security groups. This dual layer takes security to the next level as traffic must pass through multiple checks, which reduces the chances of unwanted access.

---

## Default vs Custom Network ACLs

### Similar to security groups, network ACLs use inbound and outbound rules

AWS sets up a default network ACL for every VPC in your account. This default is designed to allow all traffic to move freely until you decide to customize the rules to fit your needs.

Just like security groups, network ACLs use inbound and outbound rules to decide which data packets are allowed to enter or leave subnets:

Rule 100 Inbound allows all inbound traffic into the Public Subnet.
Rule 100 Outbound allows all traffic out of the Public Subnet.

The second line in each ruleset shows an asterisk (*) that acts as a catch-all rule in case traffic does not match any of the earlier rules. In our case, since Rule 100 already allows all traffic, the asterisk rule won't actually come into play.

This means default network ACLs allow all inbound and outbound traffic, unless customized.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-security_4faeb056)

---

## Tracking VPC Resources

---

---


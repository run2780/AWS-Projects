---
# VPC Peering
---

**Author:** Arun Srinivasan | December 2025  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325

## Project Overview  

* Difficulty: Mildly Spicy
* TIme: 90 minutes
* Key Concepts: Amazon VPC, Amazon EC2, VPC Peering connections, Elastic IP addresses

In this project, VPC Peering we're going to level up by setting up VPC Peering - we're going to play with TWO VPCs instead of one!
In this project,get ready to:
* Set up multiple VPCs.
* Create a VPC peering connection - i.e. get two VPCs to talk to each other!
* Test VPC peering with connectivity tests.

### Personal reflection
Working on this project gave me a deeper appreciation for how AWS networking concepts translate into real-world connectivity challenges. Setting up multiple VPCs and then configuring a peering connection between them felt like building bridges between isolated islands—each VPC was secure and independent, but the peering link allowed them to communicate seamlessly.
One of the most valuable lessons I learned was the importance of route tables. Initially, I assumed that once the peering connection was established, traffic would automatically flow. Discovering that bidirectional routing required explicit updates to both VPCs’ route tables was eye-opening. It reinforced how critical it is to think not just about connections, but also about the paths traffic must take to move successfully.
The hands-on validation—launching EC2 instances in each VPC and testing connectivity—was particularly satisfying. Watching the ping responses succeed after carefully configuring the routes gave me a sense of accomplishment and confidence in my troubleshooting process.

Completing this project in about 90 minutes, including documentation, showed me how far I’ve come in balancing technical execution with clear communication.
Overall, this project strengthened my understanding of VPC architecture, peering, and routing fundamentals.

---

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix withothers.

### How I used Amazon VPC in this project
* Created multiple VPCs: I set up two separate Virtual Private Clouds to simulate isolated network environments, ensuring each had its own subnets and routing components.
* Configured VPC Peering: I established a peering connection between the two VPCs, enabling secure communication using private IPv4 addresses.
* Updated route tables: I added explicit routes in both VPCs so traffic could flow bidirectionally across the peering link. This step was critical to making the connection functional.
* Launched EC2 instances: I deployed one EC2 instance in each VPC to serve as test endpoints.
* Validated connectivity: I performed bidirectional traffic tests (like pinging between instances) to confirm that the peering setup worked as expected.

What is one thing you didn't expect in this project?
One thing I didn’t expect in this project was how important it is to update the route tables in both VPCs after creating the peering connection. Without those entries, the peering link exists but traffic won’t flow 

### Mission to accomplish:

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/6.%20VPC%20Peering/game%20plan.png?raw=true)

---

### Step 1 - Set up my VPC

In this step, I will create two VPCs along with their components using the VPC creation wizard, as I need to test the connectivity between them through a VPC peering connection.

### Step 2 - Create a Peering Connection

In this step, I will create a VPC peering connection between VPC1 and VPC2 to enable communication using private IPv4 addresses.

### Step 3 - Update Route Tables

In this step, I will configure routing so that traffic from VPC1 can reach VPC2, and traffic from VPC2 can reach VPC1.

### Step 4 - Launch EC2 Instances

In this step, I will launch an EC2 instance in each VPC, which will later be used to test the VPC peering connection.

### Step 5 - Validate connectivity
In this step, I will perform bidirectional traffic tests (like pinging between instances) to confirm that the peering setup worked as expected.

---

## Step 1 - Set up my VPC - Multi-VPC Architecture
In this step, I will create two VPCs along with their components using the VPC creation wizard, as I need to test the connectivity between them through a VPC peering connection.

* Log in to your AWS Account with your IAM Admin User.
* Head to your VPC console - search for VPC at the search bar at top of your page.
* From the left hand navigation bar, select Your VPCs.
* Select Create VPC.
* select VPC and more.
* Enter the VPC name
* The VPC's IPv4 CIDR block is already pre-filled to 10.0.0.0/16 - change that to 10.1.0.0/16
* Choose number of Availability Zone as 1
* Make sure public Subnet chosen is 1 and private subnet as 0.
* Choose NAT gateway as None and VPC endpoints as None.

Repeat the steps for VPC2 creation, but change the IPV4 CIDR block to 10.2.0.0/16

Why do we need to have a unique IPV4 CIDR block for each VPC?
Each VPC must have a unique IPv4 CIDR block so the IP addresses of their resources don't overlap. Having overlapping IP addresses could cause routing conflicts and connectivity issues!

### Step 2 - Create a Peering Connection
In this step, I will create a VPC peering connection between VPC1 and VPC2 to enable communication using private IPv4 addresses.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_88727bef)

A VPC peering connection is a direct connection between two VPCs.
A peering connection lets VPCs and their resources route traffic between them using their private IP addresses. This means data can now be transferred between VPCs without going through the public internet.
Without a peering connection, data transfers between VPCs would use resources' public address - meaning VPCs have to communicate over the public internet. 

### VPC Peering - Requestor Vs Acceptor
In VPC peering, the Requester is the VPC that initiates a peering connection. As the requester, they will be sending the other VPC an invitation to connect!
In VPC peering, the Accepter is the VPC that receives a peering connection request! The Accepter can either accept or decline the invitation. This means the peering connection isn't actually made until the other VPC also agrees to it!

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_1cbb1b88)

### Step 3 - Update Route Tables
In this step, I will configure routing so that traffic from VPC1 can reach VPC2, and traffic from VPC2 can reach VPC1.

Even if your peering connection has been accepted, traffic in VPC 1 won't know how to get to resources in VPC 2 without a route in your route table! You need to set up a route that directs traffic bound for VPC 2 to the peering connection you've set up.
* Add a new route to VPC 2 by entering the CIDR block 10.1.0.0/16 as our Destination.
* Under Target, select Peering Connection.
* Select VPC 1 <> VPC 2. which is the created VPC peering connection name.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_4a9e8014)


### Step 4 - Launch EC2 Instances
In this step, I will launch an EC2 instance in each VPC, which will later be used to test the VPC peering connection.

Why did you choose not to set up key pairs?
In previous projects, setting up a key pair was a key step for learning how SSH and directly accessing an EC2 instance works.
However, we've also learnt that with EC2 Instance Connect, AWS actually manages a key pair for us! We don't need to manage key pairs ourselves. Since we've already learnt how to set up key pairs twice in the last two projects, we don't need to do it again this time.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_11111111)


---

### Step 5 - Validate connectivity
In this step, I will verify the VPC peering configuration by performing bidirectional traffic tests between the EC2 instances deployed in each VPC. 
Using EC2 Instance Connect, I will securely access the instance in VPC1 and initiate connectivity checks (such as pinging the private IP of the instance in VPC2).
This validation ensures that the peering connection is not only established but also correctly configured with the necessary route table entries, allowing seamless communication between both VPCs. 
Successful tests confirm that traffic flows in both directions, demonstrating the effectiveness of the peering setup.

#### Connect to EC2 Instance 1

In this step, I will trying to connect my EC2 instance1
I was stopped from using EC2 Instance Connect.

##### Troubleshooting Instance Connect

I was stopped from using EC2 Instance Connect as "No public IPv4 address assigned. With no public IPv4 address, you can't use EC2 Instance Connect."
keeping Disable for the Auto-assign IP address option in our EC2 instance's network settings caused this error!

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_7685490c)

## Elastic IP addresses

How the Public IPV4 error got resolved and What are Elastic IP addresses?
To resolve this error, I set up Elastic IP addresses. 
Elastic IPs are static IPv4 addresses that get allocated to your AWS account, and is yours to delegate to an EC2 instance.

An Elastic IP being static is a key word here! EC2 instances by default have dynamic IPs, which means their Public IPv4 addresses change every time they're restarted. Having an Elastic IP is like having a permanent address in a city, instead of having to move from location to location every time your instance restarts.

How did Elastic IP addresses solve the connection error?
Associating an Elastic IP address to the EC2 instance 1 resolved the error because If you want to connect to your instance over EC2 Instance Connect, then your instance must have a public IP address and be in a public subnet. This is because using EC2 Instance Connect connects to your server over the internet by default. 

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_45663498)


Now the the IPV4 address eeror got fixed and see if any other errors pops out in connecting to the instance.

1 and see if the the IPV4 address eeror got fixed and see if any other errors pops out in connecting to the instance.

### Step 7 - Test VPC Peering

In this step, I will get Instance 1 to send test messages to Instance 2. Solve connection errors until Instance 2 is able to send messages back.

---



---


---

## Troubleshooting ping issues

What was the command you ran to test VPC peering?
To test VPC peering, I ran the command ping 10.2.13.225 which is the Private IP address of MyWork VPC2

A successful ping test would validate my VPC peering connection because
When you "ping" a specific IP address, your server (in this case, Instance - MyWork VPC 1) sends a small packet of data to the target server (Instance - MyWork VPC 2), asking for a response. Ping will tell you whether you get a response back and how long it took to get a response. Connectivity established through VPC2 Private IPV4 which validates the VPC peering as the traffic did not go to internet gateway.

If you receive a response quickly, it means the connection between your computer and the other computer is good. If it takes a long time or you get no response, there might be a problem with the connection!

How did you update your instance's security group inbound rules?
I had to update my second EC2 instance's security group because this security group allow ICMP traffic from sources outside of VPC 2
Let's fix this by letting inbound ICMP traffic from VPC 1.
Select Edit inbound rules.
Select Add new rule.
Change the Type to All ICMP - IPv4.
Set the Source to traffic coming from VPC 1 - 10.1.0.0/16

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-peering_7a29d352)

---

---


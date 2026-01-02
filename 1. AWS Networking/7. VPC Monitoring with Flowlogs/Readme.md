---
# VPC Monitoring with Flow Logs

---
**Author:** Arun Srinivasan | December 2025  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325
                                                                                     
## Project Overview  

* Difficulty: It's Getting Spicy..
* TIme: 90 hours
* Key Concepts: Amazon VPC, Amazon EC2, Amazon CloudWatch

In this project "VPC Monitoring with Flow Logs", Let’s kick off network monitoring by adding tracking tools to our VPC set up
Get ready to:
* Set up two VPCs and test their peering connection.
* Set up VPC Flow Logs that collect network traffic data.
* Analyse network traffic using CloudWatch's Log Insights.

### Personal reflection
Working on this project was both challenging and rewarding. Setting up two VPCs, configuring peering, and enabling Flow Logs gave me a deeper appreciation for how AWS networking components interact. At first, I assumed enabling Flow Logs would be straightforward, but discovering the need for IAM roles and policies reminded me that AWS emphasizes security and explicit permissions at every step. 
Troubleshooting connectivity between the EC2 instances was another valuable learning moment. When my first ping test failed, I realized how critical route tables and security group rules are in determining whether traffic flows as expected. Updating the route tables after creating the peering connection felt like a breakthrough—it was satisfying to see the ping replies come through once the architecture was correctly configured.

Analyzing Flow Logs in CloudWatch gave me visibility into the network traffic I had generated. Seeing rejected and later accepted requests in the logs made the abstract idea of “monitoring” very concrete.
Overall, this project strengthened my confidence in AWS networking fundamentals. I learned to approach problems systematically: check permissions, verify routing, and use logs as a diagnostic tool.

One thing you probably didn’t expect in this project was that VPC Flow Logs couldn’t immediately write to CloudWatch without extra permissions.
At first glance, it feels like enabling flow logs should be a simple toggle—create the log group, attach the flow log, and done. But then you hit that surprise moment: AWS requires you to set up an IAM policy and role so Flow Logs can actually send data to CloudWatch.

This project took me 90 minutes to set up VPC and its components including public and private instances. Then troubleshoot for errors and establish the monitoring that we intended to in this project.

---

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to
* Created two VPCs, each with unique IPv4 CIDR blocks (10.1.0.0/16 and 10.2.0.0/16). This ensured no overlapping IP ranges, which is critical for routing and connectivity.
* Configured subnets: Each VPC had a public subnet, allowing EC2 instances to be launched with public IPs for connectivity testing.
* Launched EC2 instances: Deployed one instance in each VPC, then configured security groups to allow ICMP (ping) and SSH traffic. This gave you real network activity to monitor.
* Tested VPC peering: By connecting the two VPCs, you validated that traffic could flow between them, revisiting concepts from your earlier peering project.
* Enabled VPC Flow Logs: Attached flow logs to your VPCs to capture inbound and outbound traffic at the network interface level.
* Integrated with CloudWatch: Flow logs were sent to a CloudWatch Log Group, where you could analyze traffic patterns, blocked requests, and overall network health.

## Mission to accomplish:

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/Game%20plan.png?raw=true)

---

## In the first part of my project...

### Step 1 - Set up VPCs
In this step, I will be creating two VPC from scratch and set up monitoring for them. 
You can absolutely set up monitoring for architectures with a single VPC, but we're settings up two VPCs today so we can revise some learnings from the VPC peering project too!

### Step 2 - Launch EC2 instances
In this step, I will be launching an EC2 instance in each VPC, so we can use them to test your VPC peering connection later.

### Step 3 - Set up Logs
Monitoring VPC traffic?!
In this step, I will be setting up a way to track all inbound and outbound network traffic.
Set up a space that stores all of these records.

### Step 4 - Set IAM permissions for Logs
What are we doing in this step?
VPC Flow Logs doesn't have the permission to write logs and send them to CloudWatch... yet.
Let's give Flow Logs the permission to do both, using the power of IAM roles and policies!

---

### Step 1 - Set up VPCs - Multi-VPC Architecture
What did you launch in this step?<br>
How many subnets did you create?<br>
I started my project by launching two VPCs. We created two public Subnets. i.e, One public subnet in each VPC with no private subnets.

Why are the IPv4 CIDR blocks for VPCs 1 and 2 unique?<br>
Each VPC must have a unique IPv4 CIDR block so the IP addresses of their resources don't overlap. Having overlapping IP addresses could cause routing conflicts and connectivity issues!

---

### Step 2 - Launch EC2 instances
I also launched EC2 instances in each subnet.

We were trying to connect VPC1 instance using SSH through EC2 instance which is trying to connect to your instance over the internet. Your default Security group allows only inbound traffic from within the VPC, so traffic from the internet being cut-off.
You have to allow inbound SSH traffic on Port 22.
Edit inbound rules and Select Add rule.
For your new rule, add type as "SSH" and Source type as "Anywhere IPV4" and save rules.

For VPC2 instance, add new inbound rule to allow ICMP traffic.
Edit inbound rule and Select Add rule
For the new rule, add type as "ALL ICMP - IPV4" and source as "CIDR block of VPC instance 1" which is 10.1.0.0/16

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/VPC2_Resource%20map.png?raw=true)

---
### Step 3 - Set up Logs

What are logs?
Logs are like a diary for your computer systems. They record everything that happens, from users logging in to errors popping up. It's the go-to place to understand what's going on with your systems, troubleshoot problems, and keep an eye on who’s doing what.

What are log groups?
Think of a log group as a big folder in AWS where you keep related logs together. Usually, logs from the same source or application will go into the same log group, BUT logs are also region-specific. This means log data gets created and saved in the region it was created, although you can use CloudWatch dashboards to bring together logs from different regions.

Below is the screenshot for Flow log create page:

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/flowlog_create.png?raw=true)

---

## Step 4 - Set IAM permissions for Logs

Why did you create an IAM policy?  
VPC Flow Logs by default don't have the permission to record logs and store them in your CloudWatch log group. This policy makes sure that your VPC can now send log data to your log group!

Why are you creating an IAM role?
I'm creating an IAM role because services like VPC flow logs has to be associated with a role instead of JSON and creating an IAM role will be necessary to give our VPC flow logs the access it needs to record and upload logs.

What is a custom trust policy?
A custom trust policy is specific type of policy! They're different from IAM policies. While IAM policies help you define the actions a user/service can or cannot do, custom trust policies are used to very narrowly define who can use a role.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/custom%20trust%20policy.png?raw=true)

---

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

What are we doing in this step?
Let's generate some network traffic and see whether our flow logs can pick up on them.
We're going to generate network traffic by trying to get our instance in VPC 1 to send a message to our instance in VPC 2.
Since we're trying to get our instances to talk to each other, this means we're also testing our VPC peering setup at the same time.

#### Connectivity troubleshooting

My first ping test between my EC2 instances had no replies, which means ICMP traffic could be blocked, colud be blocked by security group/ACL network ir may be our traffic might be routed to a wrong path.
I could receive ping replies if I ran the ping test using the other instance's public IP address, which means our second instance is actually allowing ICMP traffic.

Why did the ping test using Instance 2's private address fail?
Looking at VPC 1's route table, I identified that the ping test with Instance 2's private address failed because we do not have a route in our VPC's route table that directs traffic from VPC to another.

To solve this, I set up a peering connection between my VPCs

### Step 6 - Set up a peering connection

In this step, I have created a Peering Connection and Configure Route Tables.
Why did you update your VPCs' route tables?
I also updated both VPCs' route tables so that VPC 1 can connect to VPC2 using private IP address.
Update VPC 1's route table as below
* Select the checkbox next to VPC 1's route table.
* Scroll down and click on the Routes tab.
* Click Edit routes.
* Add a new route to VPC 2 by entering the CIDR block 10.2.0.0/16 as our Destination.
* Under Target, select Peering Connection.
* Select VPC 1 <> VPC 2.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/VPC1_route%20table.png?raw=true)

What do the successful ping replies indicate?
I received ping replies from Instance 2's private IP address! This means I have successfully resolved the connectivity issue by setting up a peering architecture between VPC 1 and VPC 2!
Now, VPC1 is able to communicate with VPC2 using private IP address

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/ping%20response.png?raw=true)

---

### Step 7 - Analyze flow logs

What are we doing in this step?
Let's check out what VPC Flow Logs has recorded about your network's activity!
Review the flow logs recorded about VPC 1's public subnet.
Analyse the flow logs to get some tasty insights 

Flow logs tell us about the source and destinatiion of the network traffic, the amount of data being transferred, whether the traffic was accepted or rejected.

What does your screenshotted flow log tell you?
For example, the flow log I've captured tells us that traffic from 173.255.223.149  to 10.1.11.157 was rejected and the same was accepted later.


![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/flow%20log.png?raw=true)

---

## Step 8 - Logs Insights

Logs Insights is a CloudWatch feature that analyzes your logs. In Log Insights, you use queries to filter, process and combine data to help you troubleshoot problems or better understand your network traffic!

What was the query you ran, and what does it do?
I ran the query to get the details of number of rejected requests  and limited to 20 entries

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/7.%20VPC%20Monitoring%20with%20Flowlogs/log%20insights.png?raw=true)

---


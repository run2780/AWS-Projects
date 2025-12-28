---

# Testing VPC Connectivity

---

**Author:** Arun Srinivasan | December 2025  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325
                                                                                     
## Project Overview  

* Difficulty: Mildly Spicy
* TIme: 2 hours
* Key Concepts: Amazon VPC, Amazon EC2

In this project "Testing VPC Connectivity", we're going to level up by testing our VPC's connectivity.
Get ready to:

* Connect to your Public Server from the AWS Management Console.
* Test connectivity between your EC2 instances(Public and Private)
* Test VPC connectivity with the internet.

### Personal reflection
Working through this Amazon VPC project gave me a deeper appreciation for how cloud networking is both powerful and nuanced. At first glance, launching EC2 instances and connecting them seems straightforward, but I quickly realized that every detail in the VPC configuration—whether it’s a route table, security group, or NACL—plays a critical role in determining connectivity.<br> <br>
The entire exercise, including documentation, was completed in approximately 2 hours, reflecting both my grasp of the theoretical concepts and my ability to apply them effectively in practice. This project reinforced my confidence in working with AWS networking components and highlighted the importance of structured planning when building cloud infrastructure.

* Building confidence in fundamentals: Successfully connecting to my public server and verifying internet access reinforced my understanding of how Internet Gateways and route tables enable external communication. It was rewarding to see theory translate into practice.
* Troubleshooting as a growth opportunity: When my initial ping test failed, I had to dig into NACL rules. This experience taught me that even small misconfigurations can block traffic, and that patience and systematic troubleshooting are essential skills in cloud engineering.
* Appreciating the difference between SGs and NACLs: I didn’t expect the stateless nature of NACLs to be so impactful. Learning how they differ from stateful Security Groups gave me a clearer mental model of AWS networking.
* Efficiency through hands-on practice: Completing the setup and resolving errors within two hours showed me how practice accelerates learning. Each step—from SSH access to curl tests—helped me build confidence in applying AWS concepts quickly.
* Professional takeaway: Beyond technical skills, this project reminded me of the importance of documenting my process. Clear notes on what worked, what failed, and how I fixed issues will strengthen my portfolio and demonstrate problem-solving ability to future employers.

One thing you probably didn’t expect in this project was how subtle networking configurations inside the VPC can make or break connectivity.
NACLs are stateless, meaning they track traffic separately for each direction, unlike stateful Security Groups (SGs) where an allowed inbound request automatically permits the outbound response.

This project took me 2 hours to set up VPC and its components including public and private instances. Then troubleshoot for errors and establish the connections that we intended to in this project.

---

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix withothers.

### How I used Amazon VPC in this project

How You Used Amazon VPC in the Project?
### 1. Connected to my Public Server
* Launched an EC2 instance in a public subnet of my VPC.
* The subnet was associated with a route table that had a route to the Internet Gateway, enabling external access.
* Connected via SSH using the instance’s public IP/DNS, demonstrating secure access through the VPC’s networking setup.

### 2.Tested connectivity between EC2 instances (Public ↔ Private)
* Deployed another EC2 instance in a private subnet of the same VPC.
* Using the public server as a host, tested reachability to the private server’s private IP using ping command.
* This validated that your VPC routing, Security Groups, and NACLs were correctly configured to allow internal communication between subnets.

### 3.Verified Internet connectivity from the Public Server
* From the public EC2 instance, ran curl commands to external domains.
* Successful responses confirmed that the Internet Gateway and route table associations were functioning.

### Mission to accomplish:

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/5.%20Testing%20VPC%20Connectivity/gameplan.png?raw=true)

---

## 1. Connecting to an EC2 Instance

Connectivity is all about how well different parts of your network talk to each other and with external networks. It's essential because connectivity is how data flows smoothly across your network, powering everything from simple web hosting on the Internet to complex operations

1. Connect to MyWork Public Server
My first connectivity test was whether I could connect to my network's Public Server(EC2 instance) which is "MyWork Public Server"

#### EC2 Instance Connect

* EC2 Instance Connect is a shortcut way to get direct SSH access to your EC2 instance!
* When you use SSH to connect to an EC2 instance, you usually have to:
* Generate a key pair (public and private keys).
* Associate the public key with your EC2 instance.
* Securely store the private key on your local machine.
* Set up an SSH client (a software that can handle the SSH protocol), provide it your private key, and establish a secure connection to your EC2 instance.
* EC2 Instance Connect is an alternative way to use SSH - Instance Connect lets you securely connect to your EC2 instances directly using the AWS Management Console. You're still using SSH, but with all the key management handling it for you. This takes away a lot of the complexity of setting up SSH.

Here's how EC2 Instance Connect works:
* It generates a one-time-use SSH key pair on your behalf when you initiate a connection.
* It automatically links the public key to the EC2 instance.
* It allows the key to be used for a short period.

My first attempt at getting direct access to my public server resulted in an error, because the security group associated with MyWork Public Server lets in all inbound HTTP traffic, but this is not how we're trying to access our Public Server!
We're trying to access MyWork Public Server using SSH through EC2 Instance Connect, which is a different traffic type.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/5.%20Testing%20VPC%20Connectivity/Connect_public_server_error.png?raw=true)

I fixed this error by updating MyWork Public Server's security group so it can let in SSH traffic. Choosing Anywhere-IPv4 as the source lets in SSH connections from any IPv4 address.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/5.%20Testing%20VPC%20Connectivity/Connect_public_server_Success.png?raw=true)

---

## Connectivity Between Servers

2. Test connectivity between your EC2 instances
In this step, 
Get your Public Server to talk to your Private Server.
Troubleshoot for any connection issue.

What is Ping?
Ping is a common computer network tool used to check whether your computer can communicate with another computer or device on a network.
Think of it like sending a tiny message that says "hello, are you there?" to another computer.
When you "ping" a specific IP address address, your server (in this case, MyWork Public Server) sends a small packet of data to that address (MyWork Private Server), asking for a response. Ping will tell you whether you get a response back and how long it took to get a response.
If you receive a response quickly, it means the connection between your computer and the other computer is good. If it takes a long time or you get no response, there might be a problem with the connection!

The ping command I ran was 
Ping 10.0.1.199 which is an IPv4 address of my Private Server(EC2 instance). 

The first ping returned a single line output. 
This single line indicates that your Public Server has sent out a ping message and that's about it.
Usually, when you ping another computer successfully, you should see several replies back instantly. Each reply tells you how long it took for the message to go to the Private Server and come back.
If you don't get any replies (that's our situation right now), or if the replies stop suddenly, it's usually a sign that there's a problem with the connection. 

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-connectivity_defghijk)

---

## Troubleshooting Connectivity

How did you resolve this error?
Our route table is set up perfectly (zero issues there!), but the MyWork NACL tab shows us that all traffic inbound and outbound are denied.
This means even if our route table correctly directs the ping to MyWork Private Server, the network ACL is checking the ping traffic at the entrance of your private subnet. If it finds that ICMP traffic is not allowed, it stops the ping there.
Edit the inbound rules tab and add a new rule to let MyWork Public Server ping MyWork Private Server.
Select Add new rule.
Assign 100 as the rule number.
Change the Type to All ICMP - IPv4.
What is ICMP - IPv4?
When you set a rule for All ICMP - IPv4, you're allowing all types of ICMP messages for IPv4 addresses. This covers a wide range of operational messages that are essential for diagnosing network connectivity issues, ping requests and responses are just one type of ICMP messages.
Set the Source to traffic coming from your public subnet - 10.0.0.0/24.



![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-connectivity_4a9e8014)

---

## Connectivity to the Internet

What does curl mean?
Just like ping, curl is a tool to test connectivity in a network.

I used curl to test the connectivity between my Public Server(EC2 instance) and Internet.

### Ping vs Curl

Ping and curl are different because Ping checks if one computer can contact another (and how long messages take to travel back and forwth), curl is used to transfer data to or from a server. That means on top of checking connectivity, you can use curl to grab data from, or upload data into other servers on the internet!

---

## Connectivity to the Internet

What was the successful curl command you ran?
I ran the curl command Curl google.com which returned the complete HTML content of google.com website

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-connectivity_8ee57662)

---

---


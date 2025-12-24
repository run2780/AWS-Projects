
---

# Build a Virtual Private Cloud (VPC) 


---

**Author:** Arun Srinivasan | December 2025
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325
                                                                                     
## Project Overview  
This project focuses on the foundational aspects of AWS networking. The objective is to design and implement a Virtual Private Cloud (VPC), configure a Subnet, and establish an Internet Gateway. Through this hands-on exercise, I aim to strengthen my understanding of AWS networking concepts by building and managing a custom VPC environment.

### What is Amazon VPC?
A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.
In today's project, I created AWS to create my VPC along with Subnet and Internet gateway to demonstrate my learnings in building VPC

### Personal reflection
Completing this project provided me with valuable hands-on experience in the foundational aspects of AWS networking. I was able to deepen my understanding of key networking concepts such as IPv4 addressing, CIDR blocks, and subnetting.

The entire exercise, including documentation, was completed in approximately 45 minutes, reflecting both my grasp of the theoretical concepts and my ability to apply them effectively in practice. This project reinforced my confidence in working with AWS networking components and highlighted the importance of structured planning when building cloud infrastructure.

---
### Mission to accomplish:
![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Build%20a%20Virtual%20Private%20Cloud/VPC_architecture.png?raw=true)

---

### Virtual Private Clouds (VPCs)

##### What I did in this step?  
<br> Step: Creating an AWS IAM User  
To securely set up my VPC, I will create an AWS Identity and Access Management (IAM) user. Until now, I have been working with the AWS root user, which is automatically provided when an account is first created. The root user has unrestricted access to all resources and services, making it extremely powerful but unsuitable for everyday operations.

Because of the elevated privileges associated with the root account, AWS strongly advises against using it for routine tasks. If compromised, the root user could expose the entire account to significant risk. A more secure approach is to create dedicated IAM users with only the permissions required for their specific responsibilities. This principle of least privilege ensures tighter control, minimizes potential vulnerabilities, and enhances overall account security.

##### How VPCs work?
  
A Virtual Private Cloud (VPC) is a logically isolated section of the AWS cloud where you can provision and manage resources within a network that you fully control. It provides a secure and dedicated environment, ensuring that your resources remain separate from those of other customers. Within a VPC, you can define your own IP address ranges, create subnets, configure route tables, and establish gateways, giving you granular control over networking and security. 

#### Can I create anything in my AWS account without a VPC? 
You can use some AWS services like Amazon S3 or AWS Lambda without setting up a VPC. These services are designed to work on the internet without needing a private network setup.

##### Why there is a default VPC in AWS accounts
There was already a default VPC in my account ever since my AWS account was created. This is because AWS has set up a default VPC to allow me to deploy resources like EC2 instances or RDS databases right away without having to create my own VPC from scratch.

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Build%20a%20Virtual%20Private%20Cloud/create%20vpc.png?raw=true)

##### Defining IPv4 CIDR blocks

IPv4 stands for Internet Protocol version 4, which is the most common way to write an IP address. To set up my VPC, I had to define an IPv4 CIDR block, which means a range of IP address my VPC can allocate to the resources deployed into my VPC. What is a CIDR block? CIDR (which stands for Classless Inter-Domain Routing) is a way to assign a whole block of IP addresses, kind of like creating a zone/area in a city. To understand how big a CIDR block is, look at the number after the slash - the smaller the number, the larger the CIDR block! For example,10.0.0.0/16 means the first 16 bits of your IP address (10.0) are fixed, but the remaining 16 bits (i.e. the second half of the IP address) can be allocated however you like. Addresses within this CIDR block start at 10.0.0.0 and go up to10.0.255.255. There are 2^16 (65,536) possible IP addresses within this subnet.

---

### Subnets

##### What I did in this step  
<br> Step: Creating a Subnet  
I will create a Subnet within the Virtual Private Cloud (VPC) to logically divide the larger network space into smaller, manageable segments. Subnets enable structured planning by designating specific areas where different resources can be deployed and operated. This subdivision not only improves organization but also enhances control over network traffic, security, and resource allocation.

##### Creating and configuring subnets
If your VPC is a city, subnets are like different neighbourhoods inside your city. You use subnets to group resources with similar access rules and restrictions. Some subnets might be public areas that all resources can access (public subnets) while others are private areas with limited access (private subnets). 
Public subnets are open areas, while private subnets are restricted. Each subnet must have a unique IP range. There are already subnets existing in my account, one for every availability zone. The default VPC in your account comes with predefined subnets in each Availability Zone of a Region, which means you'll see3 subnets on your page if your Region has 3 Availability Zones. What are Availability Zones and how do they affect my VPC? Availability Zones (AZs) are clusters of data centres within an AWS Region. Each subnet in your VPC must be placed in a specific AZ. By using multiple AZs, you get backup and high availability—if one AZ fails, others keep your application running.

##### Public vs. private subnets
Subnets could be private or public. The difference between public and private subnets are: A public subnet is connected to the internet. Resources inside a public subnet can communicate with external networks. A private subnet does not have direct internet access.
You'd use it for internal resources that don’t need to be publicly accessible. Why your subnet is not considered a public subnet yet? Though we have labelled our Subnet as "Public1", it is still not a public subnet as it is not yet connected to internet gateway. For a subnet to be considered public, it has to connect to an internet gateway.

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Build%20a%20Virtual%20Private%20Cloud/create_subnet_panel.png?raw=true)

##### Auto-assigning public IPv4 addresses
Once I created my subnet, I enabled auto-assign public IPv4 addresses. By default, your resources already have private IP addresses, but this only allows internal communication within your VPC. To access the internet or be accessible from the internet, the instance would need a public IP address. When you enable auto-assign public IPv4 address for a subnet, any EC2 instance launched in that subnet will instantly get a public IP address so you won't have to create one manually - a huge time saver!

---

### Internet gateways
##### What I did in this step?
Last step in this project - let's attach your VPC with an internet gateway. This is like building a bridge (internet gateway) that links your private city (VPC) to the outside world (the internet), so your resources can communicate beyond your private space.

##### Setting up internet gateways
An internet gateway connects your city (VPC) and the outside world (internet).Internet gateways are key to making applications available on the internet. By attaching an internet gateway, your instances can access the internet and be accessible to external users.
Attaching an internet gateway means resources in your VPC can now access the internet. The EC2 instances with public IP addresses also become accessible to users, so your applications hosted on those servers become public too.

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Build%20a%20Virtual%20Private%20Cloud/internet%20gateway%20panel.png?raw=true)

---

### Using the AWS CLI

##### What I'm doing in this extension
In this project extension, I will open a handy tool (called AWS Cloud Shell) to run commands. Run AWS CLI commands to set up a VPC, subnet and internet gateway

#### Exploring Cloud Shell and CLI
##### What is Cloud Shell?
AWS Cloud Shell is shell in your AWS Management Console, which means it's a space for you to run code. The awesome thing about AWS Cloud Shell is that it already has AWS CLI pre-installed. What is CLI? AWS CLI (Command Line Interface) is a software that lets you create, delete and update AWS resources with commands instead of clicking through your console.

##### Debugging my setup
Let's create a new VPC with the CIDR block 10.0.0.0/24. To set up a VPC or a subnet, you can use the command 
“aws ec2 create-vpc --cidr-block 10.0.0.0/24 --query Vpc.VpcId --output text” 
Make sure to avoid errors by including CIDR block. Initially error came when CIDR block was not mentioned. 

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Build%20a%20Virtual%20Private%20Cloud/vpc_create_error_CLI.png?raw=true)

##### Why does the command start with “aws ec2”? Aren't we creating a VPC? 
VPCs were originally designed for setting up private networks for EC2 instances! VPCs are now essential for a wider range of services, but they were initially tied to EC2, so their CLI commands still start with “aws ec2”. Break down of this command: aws ec2tells the CLI we want to use the EC2 service.
“create-vpc” is the specific action we are doing, which is creating a new VPC.
--cidr-block 10.0.0.0/24 sets up the CIDR block for the VPC we're creating.
--queryVpc.VpcId --output text asks the terminal to format its response as plain text and only show the VPC ID (instead of all the other data is usually shows).

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Build%20a%20Virtual%20Private%20Cloud/cloudshell_CLI.png?raw=true)

### Comparing Cloud Shell vs. AWS Console
Cloud Shell is an excellent middle-ground that provides the power of the CLI without the overhead of local setup and authentication. It is particularly useful for debugging resources within private networks, potentially replacing traditional bastion hosts. The Cloud Console (GUI) is the most intuitive for visual learners and simple, one-off tasks, but it becomes cumbersome for complex or repetitive configurations. Overall, I preferred Cloud console because it is highly scriptable (Bash, Python) for repeatable tasks


---
  
 


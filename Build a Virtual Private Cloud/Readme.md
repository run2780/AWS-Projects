 # Build a Virtual Private Cloud (VPC)
                                                                                    
In this project, I will demonstrate how to create a VPC (Virtual Private Cloud), create a Subnet and an Internet gateway. I'm doing this project to learn and dive into the core of AWS networking by creating my very own VPC.

### What is Amazon VPC?
A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.
In today's project, I created AWS to create my VPC along with Subnet and Internet gateway to demonstrate my learnings in building VPC

### Personal reflection
This project took me around 35 minutes including documentation after understanding the concepts of IPV4, CIDR blocks, Subnets and Internet gateway.

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/Build%20a%20Virtual%20Private%20Cloud/VPC_architecture.png?raw=true)

### Virtual Private Clouds (VPCs)

### What I did in this step?
In this step, I will create an AWS IAM user to set up my VPC. So far, I’ve only been using the AWS root user, but that’s not safe for everyday work. When you first open an AWS account, you get access through the root user. This account has full control over everything. Because it’s so powerful, AWS recommends not using the root user for daily tasks. If someone gets access to it, they could take over your whole account. Instead, the safer way is to create an IAM user. IAM users can be given only the permissions they need, which keeps your account more secure.

### How VPCs work?
A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others. 

#### Can I create anything in my AWS account without a VPC? 
You can use some AWS services like Amazon S3 or AWS Lambda without setting up a VPC. These services are designed to work on the internet without needing a private network setup.

### Why there is a default VPC in AWS accounts
There was already a default VPC in my account ever since my AWS account was created. This is because AWS has set up a default VPC to allow me to deploy resources like EC2 instances or RDS databases right away without having to create my own VPC from scratch.

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/Build%20a%20Virtual%20Private%20Cloud/create%20vpc.png?raw=true)

### Defining IPv4 CIDR blocks

IPv4 stands for Internet Protocol version 4, which is the most common way to write an IP address. To set up my VPC, I had to define an IPv4 CIDR block, which means a range of IP address my VPC can allocate to the resources deployed into my VPC. What is a CIDR block? CIDR (which stands for Classless Inter-Domain Routing) is a way to assign a whole block of IP addresses, kind of like creating a zone/area in a city. To understand how big a CIDR block is, look at the number after the slash - the smaller the number, the larger the CIDR block! For example,10.0.0.0/16 means the first 16 bits of your IP address (10.0) are fixed, but the remaining 16 bits (i.e. the second half of the IP address) can be allocated however you like. Addresses within this CIDR block start at 10.0.0.0 and go up to10.0.255.255. There are 2^16 (65,536) possible IP addresses within this subnet.

### Subnets

#### What I did in this step
In this step, I will be creating a subnet to divide this large space (VPC) into subdivisions so you can start planning where different resources will live and operate.

#### Creating and configuring subnets
If your VPC is a city, subnets are like different neighbourhoods inside your city. You use subnets to group resources with similar access rules and restrictions. Some subnets might be public areas that all resources can access (public subnets) while others are private areas with limited access (private subnets). 
Public subnets are open areas, while private subnets are restricted. Each subnet must have a unique IP range. There are already subnets existing in my account, one for every availability zone. The default VPC in your account comes with predefined subnets in each Availability Zone of a Region, which means you'll see3 subnets on your page if your Region has 3 Availability Zones. What are Availability Zones and how do they affect my VPC? Availability Zones (AZs) are clusters of data centres within an AWS Region. Each subnet in your VPC must be placed in a specific AZ. By using multiple AZs, you get backup and high availability—if one AZ fails, others keep your application running.

#### Public vs. private subnets
Subnets could be private or public. The difference between public and private subnets are: A public subnet is connected to the internet. Resources inside a public subnet can communicate with external networks. A private subnet does not have direct internet access.
You'd use it for internal resources that don’t need to be publicly accessible. Why your subnet is not considered a public subnet yet? Though we have labelled our Subnet as "Public1", it is still not a public subnet as it is not yet connected to internet gateway. For a subnet to be considered public, it has to connect to an internet gateway.

![Image Alt](https://github.com/run2780/AWS-Projects/blob/main/Build%20a%20Virtual%20Private%20Cloud/create_subnet_panel.png?raw=true)





  
 


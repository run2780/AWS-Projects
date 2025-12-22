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

####Can I create anything in my AWS account without a VPC? 
You can use some AWS services like Amazon S3 or AWS Lambda without setting up a VPC. These services are designed to work on the internet without needing a private network setup.

### Why there is a default VPC in AWS accounts
There was already a default VPC in my account ever since my AWS account was created. This is because AWS has set up a default VPC to allow me to deploy resources like EC2 instances or RDS databases right away without having to create my own VPC from scratch.

![Image Alt][(https://github.com/run2780/AWS-Projects/blob/main/Build%20a%20Virtual%20Private%20Cloud/VPC_architecture.png?raw=true)](https://github.com/run2780/AWS-Projects/blob/main/Build%20a%20Virtual%20Private%20Cloud/create%20vpc.png?raw=true)

  
 


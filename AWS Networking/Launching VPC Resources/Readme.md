---
# Launching VPC Resources

---

**Author:** Arun Srinivasan | December 2025  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325

## Project Overview  

* Difficulty: Mildly Spicy   
* TIme: 60 min
* Key Concepts: Amazon VPC, Amazon EC2 

The goal of this project was to deploy compute resources inside a custom Amazon VPC to understand how networking, isolation, and accessibility work in AWS. Using the Amazon VPC wizard, you quickly created a new Virtual Private Cloud with both public and private subnets, route tables, and gateways.
### Key Activities
* Created a VPC with the wizard: Automated setup of subnets, route tables, and an Internet Gateway for rapid deployment.
* Launched an EC2 instance in the public subnet: This instance had internet access, making it suitable for hosting applications or services that need to be publicly reachable.
* Launched an EC2 instance in the private subnet: This instance was isolated from direct internet access, enhancing security and demonstrating how backend resources can be protected.

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.

### How I used Amazon VPC in this project

* Created a VPC using the wizard  
* Leveraged the Amazon VPC wizard to quickly spin up a new Virtual Private Cloud with default configurations, saving time compared to manual setup.
* Launched an EC2 instance in the public subnet. This instance was placed in a subnet with an Internet Gateway attached, allowing it to communicate directly with the internet. Ideal for web servers or resources that need public access.
* Launched an EC2 instance in the private subnet. This instance was placed in a subnet without direct internet access, enhancing security. It can communicate with the public subnet instance or use a NAT Gateway for outbound traffic.

#### How Amazon VPC Was Used?
* Network isolation: You created a logically isolated section of AWS where you control IP ranges, routing, and security.
* Subnet segmentation: Divided resources into public and private subnets to balance accessibility and security.
* Resource deployment: EC2 instances were launched into specific subnets, demonstrating how work.

### Personal reflection
Completing this project gave me a deeper appreciation for how Amazon VPC structures cloud networking. At first, I expected the process to be complex and time‑consuming, but the VPC wizard streamlined much of the setup, which was both surprising and encouraging.
Launching EC2 instances into both public and private subnets helped me clearly see the difference between resources that need internet access and those that should remain isolated for security. It reinforced the importance of designing architectures that balance accessibility with protection.

I also realized that while automation makes things faster, it’s still essential to understand the underlying components—like route tables, gateways, and subnets—so I can troubleshoot or customize when needed. Overall, this project boosted my confidence in working with AWS networking and gave me practical insight into how cloud environments are built for real‑world applications.

One thing I did not expected in this project was how much the VPC wizard automated for you.
Instead of manually configuring route tables, subnets, and gateways step by step, the wizard instantly provisioned those resources in a structured way. That meant you didn’t just get a blank VPC—you got a ready‑to‑use environment with public and private subnets, route tables, and an Internet Gateway already wired up.
The entire exercise, including documentation, was completed in approximately 1 hour, reflecting both my grasp of the theoretical concepts and my ability to apply them effectively in practice. This project reinforced my confidence in working with AWS networking components and highlighted the importance of structured planning when building cloud infrastructure.

#### Learning Outcomes
* Gained hands-on experience with network segmentation (public vs. private subnets).
* Understood how Internet Gateways and NAT Gateways enable or restrict connectivity.

### Mission to accomplish
![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Launching%20VPC%20Resources/game%20plan.png?raw=true)


---

## Speeding up VPC creation
I created a VPC MyWork VPC in a traditional way using AWS console. Created public and private Subnets, Internet gateway, Public Security group, public and Private Network ACL

Again, I used an alternative way to set up an Amazon VPC! This ia an addition to see how quick VPC can be set up using this wizard. Instead of manually configuring route tables, subnets, and gateways step by step, the wizard instantly provisioned those resources in a structured way.

* Head back to your VPC console.
* From the left hand navigation bar, select Your VPCs.
* Select Create VPC.
* We previously stuck to creating a VPC only, but this time select VPC and more.

A visual flow diagram pops up that shows us other VPC resources. This is called a VPC resource map!
![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Launching%20VPC%20Resources/VPC%20create%20wizard.png?raw=true)

What is a VPC resource map?
With VPC resource map, you can quickly understand the architectural layout of a VPC, like the number of subnets, which subnets are associated with which route table, and which route tables have routes to an internet gateway.
Now with this handy VPC resource map, you get to see that selecting the VPC and more option will also help you create VPC resources in the exact same page. No more jumping between pages in your VPC console!

Why can your new VPC have the same IPv4 CIDR block as NextWork VPC?
Actually, you can have multiple VPCs with the same IPv4 CIDR block in the same AWS region and account. AWS VPCs are isolated from each other by default, so there won't be any IP conflicts unless you explicitly connect them using VPC peering.
Bottom line, it's possible for your new VPC to share the same CIDR block as an existing one, but this set up will mean your overlapping VPCs can't talk to each other directly. That's why it'd be best practice to have completely unique CIDR blocks for each VPC in your account!

Resource Map of the VPC created in a traditional way and not the automated one.
![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Launching%20VPC%20Resources/resource%20map.png?raw=true)

#### Tips for using the VPC resource map

How is it that subnets can't have overlapping CIDR blocks, but VPCs can?
VPCs are isolated networks within AWS, meaning they don’t interact with each other unless you explicitly set up connectivity between them.
On the other hand, subnets within a VPC are part of the same network and can directly communicate with each other. Overlapping CIDR blocks within a VPC would create IP address conflicts, making it impossible to route traffic correctly. So, subnets need unique CIDR blocks to ensure smooth internal networking.

How many public subnets could you create?
AWS's best practice advice is having more private subnets can help with organizing your resources and isolating them for security purposes, whereas public subnets are limited to ensure manageable exposure to the internet.
Why can't I create more than two public subnets?
The VPC wizard limits you to two public subnets to keep things straightforward. If you need more, you can always add them manually later - you can have up to 200

NAT gateways let instances in private subnets access the internet for updates and patches, while blocking inbound traffic.

For example, your private server in your private subnet might need to download security updates. By using a NAT gateway, the server can access these updates securely while remaining protected from external threats!

On the other hand, internet gateways let instances in public subnets communicate with the internet both ways i.e. both inbound and outbound traffic.

---
## Setting Up Direct VM Access

Directly accessing a virtual machine means logging into and managing the operating system or software of the machine as if you were using it in front of you, but over the internet.

The AWS Management Console gives you a user-friendly interface to set up and manage AWS resources but it doesn't typically provide direct access to the operating systems of your EC2 instances. For operations that require direct OS-level access, like installing software, editing configuration files, or running scripts, you'd use your key pair to connect directly. This method of access is essential for deeper administrative tasks or specific kinds of troubleshooting that can't be performed through the console.

### SSH is a key method for directly accessing a VM

SSH, or Secure Shell, is the protocol we use for this secure access to a remote machine. When you connect to the instance, SSH verifies you possess the correct private key corresponding to the public key on the server, ensuring only authorized users can access the instance.

In terms of network communication, SSH is also as a type of network traffic. Once SSH has established a secure connection between you and the EC2 instance, all data transmitted (including your commands and the responses from the instance) is encrypted. This encryption makes SSH an ideal method for securely exchanging confidential data e.g. login credentials!

### To enable direct access, I set up key pairs

What is a key pair?
Key pairs help engineers directly access their virtual machines, like EC2 instances.

How key pairs work?
They consist of two cryptographic keys: one private and one public. The public key is installed on the virtual machine, and the private key remains with the user. When you attempt to connect, the machine uses the public key to create an encrypted challenge that can only be decrypted with the private key. Key pairs make sure that access to your EC2 instances is secure and authenticated.

What is a private key's file format, which format was your private key?

Just like how documents can be saved in various file formats like PDF, DOCX, or TXT, each suited for different applications or systems, private keys also come in different file formats. Not every system or application can process all these formats, so choosing the right one is crucial.

The .pem format, which stands for Privacy Enhanced Mail, started off as a way to secure emails but has since become the go-to format for managing cryptographic keys because it is supported by many different types of servers e.g. EC2 instances!

---

## Launching a public server

Why are we editing the Network settings?

By default, all resources are launched into the default VPC that AWS has set up for your account. We need to tell AWS that we actually want to launch this instance in MyWork VPC and the MyWork Public Subnet!

How did you edit your EC2 instance's networking settings?

I had to change my EC2 instance's networking settings by:
At the Network settings panel, select Edit at the right hand corner.
Select MyWork VPC from the drop-down in the VPC list.
Select your public subnet.
For the Firewall (security groups), we've already created the security group for your public subnet's resources. Choose Select existing security group.
Select MyWork Public Security Group.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Launching%20VPC%20Resources/Public%20server%20create.png?raw=true)

---

## Launching a private server

In this step, I will launching another EC2 instance which is a private Server to protect my EC2 instance.

Why is your private server using a different security group from your public server?
My private server using a different security group  becaue Mywork Public security group allows in all HTTP traffic which would leave our private server much more vulnerable to security risks.

Can I use the same key pair between multiple instances?
Yes, you can use the same key pair for more than one EC2 instance! This means you can use the same private key (i.e. the .pem file) to log into any of your instances using this key pair, making it easier to manage.

From a security point of view, anyone with that key can access all the instances it's connected to - making it even more important to keep your private key safe.

What is my new security group's source?
My private server security group source is my MyWork public security group which means only SSH traffic coming from resources associated with that security group would be allowed.

![Image](https://github.com/run2780/AWS-Projects/blob/main/AWS%20Networking/Launching%20VPC%20Resources/Private%20Security%20group%20settings.png?raw=true)

---


---

---


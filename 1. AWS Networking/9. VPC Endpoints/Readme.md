
---
# VPC Endpoints

---

**Author:** Arun Srinivasan | December 2025  
**Linkedin:** www.linkedin.com/in/arun-srinivasan-2a244325

## Project Overview  

* Difficulty: Mildly Spicy
* TIme: 90 minutes
* Key Concepts: Amazon VPC, Amazon EC2, VPC Peering connections, Elastic IP addresses

In this project, We will see how to use our VPC to access other AWS services, specifically S3.
But there's just one thing... with this setup, your EC2 instance is accessing your S3 bucket through the public internet.

How is that a problem?
When traffic is going through the internet, there is a higher risk of external threats and attacks looking at your data!

For example, attackers can expose your EC2 instance's keys to your AWS account if they manage to break into the traffic between your instance and S3 bucket. That's not great for security!

But we need resources to talk to each other to build things on AWS!
So how could we solve this problem and let our VPC communicate with other AWS services in a safer way?

Introducing... VPC endpoints!

VPC endpoints gives your VPC private, direct access to other AWS services like S3, so traffic doesn't need to go through the internet.
Just like how internet gateways are like your VPC's door to the internet, you can think of VPC endpoints as private doors to specific AWS services.

Get ready to:
* Use VPC endpoints to directly access your S3 bucket from your VPC.
* Test your endpoint setup using an S3 security tool called bucket policies!

## personal reflection:
Completing this project gave me a deeper appreciation for how AWS networking concepts translate into real-world security practices. At first, I thought connecting an EC2 instance to S3 was straightforward, but I quickly realized that routing traffic through the public internet introduced unnecessary risks. Setting up a VPC endpoint showed me how AWS provides elegant solutions to keep communication private and secure.
One unexpected moment was when my own access to the S3 bucket was blocked after applying the bucket policy. It was a reminder that security configurations can be powerful but also unforgiving if not carefully planned. This taught me the importance of testing policies step by step and validating access from different perspectives.

Overall, the project strengthened my confidence in working with VPCs, endpoints, and policies. It showed me how small misconfigurations can block access entirely, but also how precise adjustments can restore secure connectivity. 

#### Key takeaways:

* Security awareness: realizing the risks of public internet traffic.
* Hands-on troubleshooting: resolving denied access after bucket policy changes.
* Best practices: understanding why IAM roles are preferred over access keys.
* Confidence building: gaining trust in my ability to configure endpoints and policies.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to set up VPC Endpoint , Specifically S3 gateway. This provides my VPC with direct access to another AWS service.
This project took me around 90 minutes including documentation.
---


### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.

## In the first part of my project...

### Step 1 - Architecture set up and Connect to EC2 instance

What are we doing in this step?
In this project, I will be setting up the foundations of this project; i.e, launching a VPC, EC2 instance and S3 bucket and test the set up in this last step of the networking project.
I will be connecting directly to EC2 instance using EC2 instance connect. Connecting to my EC2 instance will help us in accessing S3 and run commands later in this project.

### Step 2 - Set up access keys

What are we doing in this step?
In this step, I will set up an access key so that my EC2 instance will have access to AWS environment.

### Step 3 - Interact with S3 bucket

In this step, I will be applying my access key credentials to my EC2 instance and then will be using AWS CLI and EC2 instance to access Amazon S3.

---

### Step 1 - Architecture set up and connect to EC2 instance

What resources did you launch in this step?
I started my project by launching three key resources, a VPC, an EC2 instance and S3 bucket.

What did you set up in this step?
I also set up Amazon S3 bucket and uploaded 2 files in it to access them later in this project

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/objects%20in%20S3.png?raw=true)

---

### Access keys

What were the four things you just configured?
To set up my EC2 instance to interact with my AWS environment, I configured AWS access key id, secret access key, default region and default output format.

What are access keys?
An access key ID is a part of a credential!
Your credentials are made up of a username and password; think of the access key ID as the username.
You don't automatically have one, but you can create access keys IDs through AWS IAM.

What is a secret access key?
The secret access key is like the password that pairs with your access key ID (your username). You need both to access AWS services.

Secret is a key word here - anyone who has it can access your AWS account, so we need to keep this away from anyone else!

### Best practice

What is the best practice alternative to using access keys?
Although I'm using access keys in this project, a best practice alternative is to use IAM role.
we're creating access keys and manually applying them in our EC2 instance, but typically the recommended way is to create an IAM role with the necessary permissions and then attaching that role to your EC2 instance.

Your EC2 instance would inherit the permissions from the role, and this is best practice as you can easily attach and detach EC2 instances from roles to give and take away their credentials.
We aren't using this method so that we can learn about access keys, but roles are usually a better alternative for security.

---

## Connecting to my S3 bucket

What was the command you ran?
The command I ran was 'aws s3 ls'. This command is used to list all S3 buckets in the AWS account.

What did your terminal respond with?
When I ran the command 'aws s3 ls' again, the terminal responded with the list of s3 buckets. This indicated that the access key is successfully configured to connect to AWS environment from EC2 instance instead of AWS management console.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/aws%20s3%20ls%20output.png?raw=true)

---

What was the next command you ran?
I also tested the command 'aws s3 ls s3://mywork-run2780 ' which returned the list of objects in my bucket.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_4334d779)

---

## Uploading objects to S3

What was the first command you ran?
To upload a new file to my bucket, I first ran the command 'sudo touch /tmp/test.txt'.
This command creates a text file named 'test' in /tmp folder.

The second command I ran was 'aws s3 cp /tmp/test.txt s3://mywork-vpc-project-arun'.
This command will copy the text file test form the /tmp folder to my s3 bucket 'mywork-vpc-project-arun'

The third command I ran was 'aws s3 ls' which validated that the test file 'test.txt' is copied to my s3 bucket 'mywork-vpc-project-arun'.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/objects%20in%20my%20bucket.png?raw=true)

---

## In the second part of my project...

### Step 4 - Set up a Gateway

In this step, I will be setting up VPC Endpoint so that communication between our VPC and other services, specifically S3 is direct and secure.

### Step 5 - Bucket policies

What are we doing in this step?
In this step, we are testing our Endpoint connection by blocking off all traffic to our Amazon S3 bucket except for traffic coming from our Endpoint.

### Step 6 - Update route tables

 What are we doing in this step?
In this step, I will be testing my endpoint connections between my bucket and EC2 instance.

### Step 8 - Validate endpoint conection

What are we doing in this step?
In this step, I will be validating VPC endpoint setup one more time. I'm also going to use Endpoint policies to restrict EC2 instance's access to our AWS environment.

---

### Setting up a Gateway

What is a Gateway?
I set up S3 gateway, which is a type of endpoint used specifically for Amazon S3 and DynamoDB (DynamoDB is an AWS database service).
Gateways work by simply adding a route to your VPC route table that directs traffic bound for S3 or DynamoDB to head straight for the Gateway instead of the internet.

### What are endpoints?

What is an endpoint?
An endpoint in AWS is a service that allows private connections between your VPC and other AWS services without needing the traffic to go over the internet.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/endpoint_create.png?raw=true)

---

## Bucket policies

What is a bucket policy?
A bucket policy is a type of IAM policy designed for setting access permissions to an S3 bucket. Using bucket policies, you get to decide who can access the bucket and what actions they can perform with it.

What does your bucket policy do?
My bucket policy denies all actions (s3:*) on your S3 bucket and its objects to everyone (Principal: "*")... unless the access is from the VPC endpoint with the ID defined in aws:sourceVpce.
In other words, only traffic coming from your VPC endpoint can get any access to your S3 bucket!

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/bucket%20policy.png?raw=true)

---

## Bucket policies

Why are you seeing denied access warnings?
Right after saving my bucket policy, my S3 bucket page showed 'denied access' warnings. This was because my policy denies all actions unless they come from your VPC endpoint. This means any attempt to access my bucket from other sources, including the AWS Management Console, is blocked!

Why did you update your route table?
I also had to update my route table because my Route table by default did not provide a route for traffic in my public subnet to the VPC endpoints.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/bucket_error.png?raw=true)

---

## Route table updates

What did you do to update your route table?
To update my route table, I visited the endpoints page of my VPC console and modified the route table from there to associate our VPC's public subnet.

What was the terminal's response?
After updating my public subnet's route table, my EC2 instance could connect with S3 bucket. Access was no longer denied!

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/endpoint_route%20table_added.png?raw=true)

---

## Endpoint policies

What is an endpoint policy?
An endpoint policy is a type of policy designed for specifyign a range of resources and actions permitted by an endpoint.

I updated my endpoint's policy by changing the effect from 'Allow' to 'Deny'.
I could see the effect of this right away, because my EC2 instance was again denied access to S3 when i try to run another AWS S3 command.

![Image](https://github.com/run2780/AWS-Projects/blob/main/1.%20AWS%20Networking/9.%20VPC%20Endpoints/s3%20connect%20error_after%20bucket%20policy.png?raw=true)

---



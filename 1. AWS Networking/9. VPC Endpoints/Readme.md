
---
# VPC Endpoints

---

**Author:** Arun Srinivasan  
**Email:** run2780@gmail.com

---


![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_09bcaa8a)

---

## Introducing Today's Project!

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix with others.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to set up VPC Endpoint , Specifically S3 gateway. This provides my VPC with direct access to another AWS service.

### One thing I didn't expect in this project was...

What is one thing you didn't expect in this project?
One thing I didn't expect in this project was to see my own access to S3 bucket got blocked once I saved my bucket's new policy to block all traffic except traffic from my Endpoint.

### This project took me...

How much time did this project take you?
This project took me around 90 minutes including documentation.

---

## In the first part of my project...

### Step 1 - Architecture set up

What are we doing in this step?
In this project, I will be setting up the foundations of this project; i.e, launching a VPC, EC2 instance and S3 bucket and test the set up in this last step of the networking project.

### Step 2 - Connect to EC2 instance

What are we doing in this step?
In this step, I will be connecting directly to EC2 instance using EC2 instance connect. Connecting to my EC2 instance will help us in accessing S3 and run commands later in this project.

### Step 3 - Set up access keys

What are we doing in this step?
In this step, I will set up an access key so that my EC2 instance will have access to AWS environment.

### Step 4 - Interact with S3 bucket

In this step, I will be applying my access key credentials to my EC2 instance and then will be using AWS CLI and EC2 instance to access Amazon S3.

---

## Architecture set up

What resources did you launch in this step?
I started my project by launching three key resources, a VPC, an EC2 instance and S3 bucket.

What did you set up in this step?
I also set up Amazon S3 bucket and uploaded 2 files in it to access them later in this project

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_4334d777)

---

## Access keys

### Credentials

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

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_4334d778)

---

## Connecting to my S3 bucket

What was the command you ran?
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

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_3e1e79a2)

---

## In the second part of my project...

### Step 5 - Set up a Gateway

In this step, I will be setting up VPC Endpoint so that communication between our VPC and other services, specifically S3 is direct and secure.

### Step 6 - Bucket policies

What are we doing in this step?
In this step, we are testing our Endpoint connection by blocking off all traffic to our Amazon S3 bucket except for traffic coming from our Endpoint.

### Step 7 - Update route tables

 What are we doing in this step?
In this step, I will be testing my endpoint connections between my bucket and EC2 instance.

### Step 8 - Validate endpoint conection

What are we doing in this step?
In this step, I will be validating VPC endpoint setup one more time. I'm also going to use Endpoint policies to restrict EC2 instance's access to our AWS environment.

---

## Setting up a Gateway

What is a Gateway?
I set up S3 gateway, which is a type of endpoint used specifically for Amazon S3 and DynamoDB (DynamoDB is an AWS database service).

Gateways work by simply adding a route to your VPC route table that directs traffic bound for S3 or DynamoDB to head straight for the Gateway instead of the internet.

### What are endpoints?

What is an endpoint?
An endpoint in AWS is a service that allows private connections between your VPC and other AWS services without needing the traffic to go over the internet.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_09bcaa8a)

---

## Bucket policies

What is a bucket policy?
A bucket policy is a type of IAM policy designed for setting access permissions to an S3 bucket. Using bucket policies, you get to decide who can access the bucket and what actions they can perform with it.

What does your bucket policy do?
My bucket policy denies all actions (s3:*) on your S3 bucket and its objects to everyone (Principal: "*")... unless the access is from the VPC endpoint with the ID defined in aws:sourceVpce.

In other words, only traffic coming from your VPC endpoint can get any access to your S3 bucket!

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_7316a13d)

---

## Bucket policies

Why are you seeing denied access warnings?
Right after saving my bucket policy, my S3 bucket page showed 'denied access' warnings. This was because my policy denies all actions unless they come from your VPC endpoint. This means any attempt to access my bucket from other sources, including the AWS Management Console, is blocked!

Why did you update your route table?
I also had to update my route table because my Route table by default did not provide a route for traffic in my public subnet to the VPC endpoints.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_4ec7821f)

---

## Route table updates

What did you do to update your route table?
To update my route table, I visited the endpoints page of my VPC console and modified the route table from there to associate our VPC's public subnet.

What was the terminal's response?
After updating my public subnet's route table, my EC2 instance could connect with S3 bucket. Access was no longer denied!

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_d116818e)

---

## Endpoint policies

What is an endpoint policy?
An endpoint policy is a type of policy designed for specifyign a range of resources and actions permitted by an endpoint.

I updated my endpoint's policy by changing the effect from 'Allow' to 'Deny'.
I could see the effect of this right away, because my EC2 instance was again denied access to S3 when i try to run another AWS S3 command.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-endpoints_3e1e79a3)

---

---

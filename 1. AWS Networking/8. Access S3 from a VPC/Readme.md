---
# Access S3 from a VPC

---

**Author:** Arun Srinivasan  
**Email:** run2780@gmail.com



![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-s3_3e1e79a2)

---

## Introducing Today's Project!

### What is Amazon VPC?

A VPC (Virtual Private Cloud) is like your own private space inside AWS. In this space, you can set up and run AWS resources in a network that you control. The main idea is that it’s separate and secure, so your resources don’t mix withothers.

### How I used Amazon VPC in this project

How did you use Amazon VPC in today's project?
In this project, I used Amazon VPC as the foundational network environment that allowed my EC2 instance to securely interact with Amazon S3.
Created a custom VPC with a CIDR block (10.0.0.0/16) to host my resources, ensuring isolation from other networks.
Configured a public subnet within the VPC so your EC2 instance could have internet access and connect via EC2 Instance Connect.
Launched an EC2 instance inside the VPC, inheriting the VPC’s routing and security rules.
Attached a security group that was tied to the VPC that allowed inbound SSH traffic, enabling you to connect to the instance.
Enabled public IP assignment since EC2 Instance Connect requires a public IP.
Provided connectivity to external AWS services: Even though S3 lives outside of VPCs, my EC2 instance inside the VPC was able to reach it over the internet once credentials were configured.

### One thing I didn't expect in this project was...

What is one thing you didn't expect in this project?
One thing you probably didn’t expect in this project was how Amazon S3 lives outside of your VPC.

At first glance, it feels natural to assume that all AWS services would sit inside the VPC you carefully designed. But S3 (along with services like IAM, Route 53, and DynamoDB) is actually a global service accessible over the internet, not bound to your VPC’s private network. That’s why you had to configure access keys and use the AWS CLI from your EC2 instance to reach it.

### This project took me...

How much time did this project take you?
This project took me 60 minutes including documentation.

---

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I will be creating a VPC and an EC2 instance because we are going to interact with S3 bucket from our EC2 instance.

### Step 2 - Connect to my EC2 instance

In this step, I will be connecting to EC2 instance, whcih is Instance-MyWork using EC2 instance connect.

### Step 3 - Set up access keys

In this step, I will creating an Access key because my EC2 instance needs credentials to access the AWS services.

---

## Architecture set up

I started my project by launching VPC and created 1 public Subnet and no private subnet. The launched EC2 instance using EC2 instance connect.

What did you set up in this step?
In this step, I launch an Amazon S3 bucket with 2 files inside. The S3 bucket will be accessed by my EC2 instance later in this project so we can test whether my access key has successfully given AWS access to my EC2 instance.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-s3_4334d777)

---

## Running CLI commands

What is AWS CLI?
AWS CLI is a special software called the AWS CLI (Command Line Interface) that you install and run on your computer to control AWS services directly from the command line i.e. your terminal! You can install this in your local computer too, and all EC2 instances come with it already installed.

What was the first command you ran?
The first command I ran was 'aws s3 ls'.
This command is used to list all the s3 buckets in the AWS account(that the EC2 instance/application has access to).

The second command I ran was 'aws configure'. This command is used to set up my EC2 instance's credentials in order to access my AWS environment.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-s3_e7fa8776)

---

## Access keys

### Credentials

What was the purpose of the 'aws configure' command?
To set up my EC2 instance to interact with my AWS environment, I configured an access key ID, secret key, default region and default output format.

What are access keys?
An access key ID is a part of a credential!

Your credentials are made up of a username and password; think of the access key ID as the username.
You don't automatically have one, but you can create access keys IDs through AWS IAM.

What is the secret access key?
The secret access key is like the password that pairs with your access key ID (your username). You need both to access AWS services.

Secret is a key word here - anyone who has it can access your AWS account, so we need to keep this away from anyone else!

### Best practice

What is the best practice alternative to using access keys?
we're creating access keys and manually applying them in our EC2 instance, but typically the recommended way is to create an IAM role with the necessary permissions and then attaching that role to your EC2 instance.

Your EC2 instance would inherit the permissions from the role, and this is best practice as you can easily attach and detach EC2 instances from roles to give and take away their credentials.

We aren't using this method so that we can learn about access keys, but roles are usually a better alternative for security.

---

## In the second part of my project...

### Step 4 - Set up an S3 bucket

What are we doing in this step?
In this step, let's create a bucket in Amazon S3.
After creating this bucket, we'll learn how to access it from our EC2 instance and do things like checking what objects are in the bucket.

### Step 5 - Connecting to my S3 bucket

What are we doing in this step?
In this step, I will be using AWS CLI command to try control/manage my s3 bucket. This means we will be interacting with our s3 bucket through our EC2 instance/VPC instead of AWS management console.

---

## Connecting to my S3 bucket

What was the first command you ran?
The first command I ran was 'aws s3 ls'.
This command is used to list all the s3 buckets in the AWS account(that the EC2 instance/application has access to).

What did your terminal respond with?
When I ran the command 'aws s3 ls' again, the terminal responded with the list of s3 buckets. This indicated that the access key is successfully configured to connect to AWS environment from EC2 instance instead of AWS management console.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-s3_4334d778)

---

## Connecting to my S3 bucket

What was the command you ran?
Another CLI command I ran was 'aws s3 ls s3://mywork-vpc-project-arun' which returned the list of objects in my S3 bucket 'mywork-vpc-project-arun'.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-s3_4334d779)

---

## Uploading objects to S3

What was the first command you ran?
To upload a new file to my bucket, I first ran the command 'sudo touch /tmp/test.txt'.
This command creates a text file named 'test' in /tmp folder.

What was the second command you ran?
The second command I ran was 'aws s3 cp /tmp/test.txt s3://mywork-vpc-project-arun'.
This command will copy the text file test form the /tmp folder to my s3 bucket 'mywork-vpc-project-arun'

What was the third command you ran?
The third command I ran was 'aws s3 ls' which validated that the test file 'test.txt' is copied to my s3 bucket 'mywork-vpc-project-arun'.

![Image](http://learn.nextwork.org/courageous_brown_peaceful_mermaid/uploads/aws-networks-s3_3e1e79a2)

---

---


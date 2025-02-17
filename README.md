# AWS CloudFormation Template for VPC, EC2, Lambda, and SNS

## Overview

This CloudFormation template automates the deployment of an AWS infrastructure that includes:

- A **VPC** with a subnet, route table, and internet gateway.
- An **EC2 instance** with SSH access and SNS publishing permissions.
- Two **Lambda functions** that process SNS messages and log them to CloudWatch.
- An **SNS topic** for event-driven communication.
- Security groups and IAM roles for access control.

## Architecture

The CloudFormation template sets up the following:

1. **VPC & Networking:**

   - Creates a VPC (10.0.0.0/16) with a subnet (10.0.1.0/24).
   - Adds an **Internet Gateway (IGW)** to allow external access.
   - Configures a **Route Table** for internet-bound traffic (0.0.0.0/0).

2. **EC2 Instance:**

   - Runs in the VPC subnet and has SSH access enabled.
   - Has permission to publish messages to the SNS topic.
   - Can be used for manual message testing and processing.

3. **SNS Topic:**

   - Acts as a messaging service to trigger the Lambda functions.
   - Allows Lambda to subscribe and receive event notifications.

4. **Lambda Functions:**

   - Two Lambda functions are deployed to process SNS messages.
   - Each function logs received messages to **CloudWatch Logs**.

5. **IAM Roles & Security:**

   - IAM roles for EC2 and Lambda functions with necessary permissions.
   - Security Groups to control inbound/outbound traffic.

## Deployment Steps

1. **Launch the CloudFormation Template:**

   - Navigate to AWS **CloudFormation Console** → Create Stack → Upload Template.
   - Provide required parameters like **Key Pair** for EC2 SSH access.
   - Deploy the stack and wait for successful creation.

2. **Test EC2 & SNS Communication:**

   - SSH into the EC2 instance:
     ```bash
     ssh -i your-key.pem ec2-user@<EC2-Public-IP>
     ```
   - Publish a test message to SNS:
     ```bash
     aws sns publish --topic-arn <SNS_TOPIC_ARN> --message "Test Message from EC2"
     ```
   - Check **CloudWatch Logs** to verify if Lambda functions received and processed the message.

3. **Create an SNS VPC Endpoint:**

   - If private communication is needed, manually create an **SNS VPC Endpoint** and test Lambda’s access without internet connectivity.

## Troubleshooting

- If messages do not appear in **CloudWatch Logs**, check:
  - **SNS topic subscriptions** (ensure Lambda is subscribed).
  - **IAM roles and permissions** (Lambda should have SNS invoke permission).
  - **EC2 instance IAM role** (should allow SNS publishing).
  - **VPC and routing** (ensure correct internet gateway and route table setup).

## Conclusion

This CloudFormation setup automates the provisioning of a scalable AWS environment that integrates EC2, Lambda, SNS, and CloudWatch for efficient message processing and logging.


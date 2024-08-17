# AWS
Amazon service explanation

Table of contents
# Steps to Set Up the Project
# Step 1: Create IAM Role for Lambda
# Step 2: Create Lambda Functions
# Step 3: Create CloudWatch Events Rules

 <! -- -->**Steps to Set Up the Project**
Create IAM Role for Lambda
Create Lambda Functions
Create CloudWatch Events Rules

-->Step 1: Create IAM Role for Lambda
Navigate to IAM:
Go to the AWS Management Console and open the IAM service.
--> Create a New Role:
Click on "Roles" in the left-hand menu.
Click "Create role."
Choose "Lambda" as the service that will use this role.
Click "Next: Permissions."

--> Attach Policies:
Attach the following policies to allow Lambda to start and stop EC2 instances:
*AmazonEC2FullAccess
*AWSLambdaBasicExecutionRole
Click "Next: Tags" (you can skip adding tags).
Click "Next: Review."
Give the role a name (e.g., LambdaEC2ControlRole).
Click "Create role."

Step 2: Create Lambda Functions
--> Create the Start EC2 Lambda Function
Navigate to the Lambda service in the AWS Management Console.
Click "Create function."
Choose "Author from scratch."
Enter the function name (e.g., StartEC2Instance).
Choose "Python 3.8" as the runtime.
Choose the role created in Step 1 (LambdaEC2ControlRole).
Click "Create function."

**Replace the default code with the following**
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    instance_id = 'i-1234567890abcdef0'  # Replace with your EC2 instance ID
    ec2.start_instances(InstanceIds=[instance_id])
    print(f'Started EC2 instance: {instance_id}')
Click "Deploy."
--> Create the Stop EC2 Lambda Function
Repeat the same steps as above to create another Lambda function, but name it StopEC2Instance.
Replace the default code with the following:

import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    instance_id = 'i-1234567890abcdef0'  # Replace with your EC2 instance ID
    ec2.stop_instances(InstanceIds=[instance_id])
    print(f'Stopped EC2 instance: {instance_id}')
Click "Deploy."

Step 3: Create CloudWatch Events Rules
--> Create a Rule to Start EC2 at 9:00 PM IST
Navigate to the CloudWatch service in the AWS Management Console.
Click "Rules" under "Events" in the left-hand menu.
Click "Create rule."

Set the event source to "Event Source" and choose "Schedule."
Set the schedule expression to trigger at 9:00 PM IST:
Use the cron expression: 30 15 * * ? *
This cron expression translates to 9:00 PM IST (IST is UTC+5:30, so 9:00 PM IST is 3:30 PM UTC).
Click "Add target."
Choose "Lambda function" and select the StartEC2Instance function.
Click "Configure details."
Give the rule a name (e.g., StartEC2At9PM).
Click "Create rule."

-->Create a Rule to Stop EC2 at 9:10 PM IST
Repeat the same steps as above to create another rule, but set the schedule expression to trigger at 9:10 PM IST:
Use the cron expression: 40 15 * * ? *
This cron expression translates to 9:10 PM IST (IST is UTC+5:30, so 9:10 PM IST is 3:40 PM UTC).
Choose the StopEC2Instance Lambda function as the target.
Give the rule a name (e.g., StopEC2At9:10PM).
Click "Create rule."

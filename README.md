# AWS-VirtualMachine-Migration-Project 

**Description:**

In this project, We have migrate virtual machine from our local to aws. consider we have laptop with windows11. now we export our win11 os into .ova extension. Then .ova extension is uploaded into s3 bucket. Then by using VMIE(Virtual Machine Import Export) service we can transfer .ova from s3 into AMI. then we can convert AMI into EC2.

**Step 1: To get .ova extension**

Install vmware workstation in your laptop.
Download one os setup from internet and configure it on the vmware.
open vmware->right click vm->shutdown vm->again right click vm->select export to .ova
it will take some minutes to save in your laptop.

**Step 2 : upload .ova into s3 bvucket**

Create an empty bucket from aws console.
Upload your .ova file extension into your s3.

**Step 3: Configure AWS Cli and give particular access.**

Install cli on your computer.(because we cant use VMIE service in now a days in aws)
Create one user and give adminaccess to user to access s3,AMI,Ec2
Login that user into cli( Use aws configure command  and give access key to login)

**Step 4:Convert .ova into AMI using CLI.**

Use below commands,

-->If you don’t already have a role for VM Import/Export, you’ll need to create one. The role should have the necessary permissions to access the S3 bucket and create EC2 instances.
aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy document "file location" (file is attached inthis repo. you can download it.)

-->Now, you can use the aws ec2 import-image command to import the .ova file from S3 into EC2.(container.json file is attached in this repo)
aws ec2 import-image --description "Your VM Description" \
  --disk-containers "file://containers.json"

-->The import process might take some time, depending on the size of your .ova file. You can monitor the status of the import by running:

aws ec2 describe-import-image-tasks --import-task-ids import-ami-xxxxxxxx

-->Replace import-ami-xxxxxxxx with the actual import task ID that was returned when you initiated the import.

**Step 5: Convert AMI into EC2** 

Go to aws console->search AMI->Select AMI->Convert into EC2.




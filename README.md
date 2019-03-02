# RStudio-on-AWS
This tutorial explains you how to run RStudio using AWS


## 1/ CREATE A KEY PAIR
Give a “key pair name” and keep the file downloaded

## 2/ CREATE A VPC
On VPC Dashboard: click “Start VPC Wizard”
Step 1: Select “VPC with a Single Public Subnet”
Step 2:
-	Give a name to the VPC
-	Optional: choose an availability zone

:arrow_right:	Your VPC is created 

## 3/ CREATE A SECURITY GROUP
On VPC dashboard click on “Security Groups” and “Create Security Group”
Step 1:
-	Give “Name tag” and “Description”
-	Choose the VPC created
Step 2: 
-	Select your security group
-	Click on the tab “Inbound Rules” and “Edit”
-	Add “SSH” type and type “ 0.0.0.0/0” in “Source”
-	Add “Custom TCP Rule”. Type “8787” in “Port Range” and “ 0.0.0.0/0” in “Source”

:arrow_right:	Your Security Group is created

## 4/ LAUNCH AN INSTANCE
On EC2 Dashboard: click “Launch instance”
Step 1: Select the first Amazon Machine Image “Amazon Linux AMI”
Step 2: Choose t2.micro instance type
Step 3:
-	Choose a network: the VPC created previously
-	Subnet: Public subnet of the VPC
-	Auto-Assign Public IP: Enable
-	Advanced details: on user data, tick “As text” and fill with the following script

#!/bin/bash
#install R
yum install -y R
#install RStudio-Server 1.0.153 (2017-07-20)
wget https://download2.rstudio.org/rstudio-server-rhel-1.0.153-x86_64.rpm
yum install -y --nogpgcheck rstudio-server-rhel-1.0.153-x86_64.rpm
rm rstudio-server-rhel-1.0.153-x86_64.rpm
#add user(s)
useradd Alexa
echo Alexa:amazon | sudo chpasswd

Step 5: Add Tags (optional)
Step 6: Tick “Select an existing security group” and choose the security group created previously

:arrow_right:	Your instance is created

## 5/ LAUNCH RSTUDIO
On EC2 Dashboard go to “Instances”
-	Select your instance
-	On the tab “Description”, copy the “IPv4 Public IP” and paste it on your browser adding “:8787” at the end
Example: XX.XXX.XXX.XXX:8787
-	A “Sign in to RStudio” page should open
-	Enter the Username and Password (according to the script, here it is “Alexa” and “amazon”) and sign in

:+1+	Enjoy RStudio ! :)

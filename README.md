I found the Cloud Interesting and while looking for practise my AWS skills. I'm doing the Project How to Create Web Server and Connect with RDS.

Overview:
In this Project we are going to Create Web Server and Connect with RDS. we are going to install an Apache web server with PHP and using MYSQL database as a backend. Both the Amazon EC2 instance and the DB instance run in a virtual private cloud (VPC) based on the Amazon VPC service.
Here I mentioned Step by Step Process, How I Created Everything.

Architecture (Diagram)overview:

AWS Services used:
•	EC2 instance.
•	MySQL database.
•	VPC (create own VPC)
•	Subnet Groups
•	Security Groups.

i) Create a own VPC
Steps:
1.	Go to VPC Service
2.	Click on Create VPC
3.	Enter the Name & IPV4 CIDR block
4.	Click on Create VPC

ii)Create Public Subnet for Web server
Steps:
1.	Click on Create Subnet
2.	Select VPC      Note : Here Select the VPC which we created
3.	Enter Subnet Name
4.	Select Availability Zone (ap-south-1a)
5.	Enter IPV4 CIDR Block
6.	Click on Create Subnet
7.	Enable Public IP  Note : We Enabling Public IP  for Subnet so that the Which Ever the Ec2 machine we                  
                                        create will have Public Ip
8.	Check Enable Auto Assign Public IPV4 Address
9.	Click on Save

iii) Create Private Subnet for Database 
1.	Click on Create Subnet
2.	Select the VPC Note : Here Select the VPC which we created
3.	Enter the Subnet Name
4.	Select Availability Zone (ap-south-1a)
5.	Enter IPV4 CIDR Block
6.	Click on Create Subnet

iv) Create Second Private Subnet 
1.	Click on Create Subnet
2.	Select the VPC Note : Here Select the VPC which we created
3.	Enter the Subnet Name
4.	Select Availability Zone (ap-south-1b)   Note : Here we are Selecting Different Availability
5.	Enter IPV4 CIDR Block
6.	Click on Create Subnet

Create a Security Group for Both Database instance and Ec2 Instance:
Note :In Ec2 instance only where we are going to host the Public Web Server, Please keep that in Mind that will avoid confusion why I'm saying adding security group to EC2 machine why I’m not saying adding security group to Public web server, both are same.

v) Creating Security Group For Ec2 Instance:
steps:
1.	Go to Security group
2.	Click on create Security group
3.	Enter the Name & Description
4.	Select the VPC  Note : Here Select the VPC which we created
5.	Click on Add for inbound rules
6.	Select type as SSH & Source as My IP
7.	Select type as HTTP & Source Anywhere IPV4
8.	Click on Create security group
9.	Note down the security group ID

vi) Create a Security group for Database Instance:
Steps:
1.	Click on Create Security group
2.	Enter the Name & Description
3.	Select the VPC      Note : Here Select the VPC which we created
4.	Click on add for inbound rules
5.	Select type as MYSQL/Aurora & Source as web server Security group
6.	Click on Create Security group

vii) Create Internet Gateway
1.	Click on Create Internet gateway
2.	Enter the name
3.	Click on Create Internet gateway
4.	Attach Internet gateway with VPC  Note : Attach to the VPC we Created not Default one.
5.	Click on Attach internet gateway

vii) Attach Route table with Private Subnets
Note: When we create the VPC by default system will create the route table

1.	Select the route table ( Our VPC Route table)
2.	Select both the Private Subnet
3.	Click on save associations

viii) Create Route table  
Note: Here we are Creating Separate Route so that we can connect Public subnet to internet gateway

1.	Click on create route table
2.	Enter the Name
3.	Select the VPC  (Select our VPC which we Created)
4.	Click on create route table
5.	Attach Public Subnet
6.	Click on save associations
7.	Attach internet gateway with route table
8.	Click on add route
9.	Attach Internet gateway as target & 0.0.0.0/0 as Destination
10.	Click on save changes

Before Creating of the Database we have to create Subnet Groups Because without Subnet Group we cannot Achieve Multi -Availability Concept

ix) Create Database Subnet Groups
1.	Database Subnet Groups are collection of subnets
2.	Go to Amazon RDS
3.	Click on Subnet Groups
4.	Click on Create DB Security group
5.	Enter the Name & Description
6.	Select the VPC
7.	Select the Availability Zone
8.	Select both the Private Subnets
9.	Click on Create

x) Create MYSQL Database
1.	Select standard create
2.	Click on create database
3.	Select engine option as MYSQL
4.	Select template as Free Tier
5.	Enter Database Instance identifier
6.	Enter Master Username
7.	Enter master password & Confirm Password
8.	Go to Connectivity
9.	Select the VPC & Select the Subnet group
10.	Select our VPC Security group
11.	Click on Additional Configuration
12.	Enter Initial Database Name    Note: Very Important. With this name we are connecting database with 
webserver) (sample)
13.	Click on Create Database

xi) Create EC2 Machine in Public Subnet

1.	Give a Name to Ec2 machine
2.	Select AMI (Amazon Machine Image) as Amazon Linux
3.	Select Instance type as t2.micro
4.	Select our VPC & Select the Public Subnet
5.	Select the Security group
6.	Launch the Instance

After that we have to Configure, for that we have to connect to EC2 machine
Please Follow the Steps as mentioned.

Connect the Instance using putty
sudo yum update -y    (Update the Software)

sudo amazon-linux-extras install -y lamp-mariadb10.2- php7.2 php7.2 (Install the PHP software)

sudo yum install -y httpd (Install the Apache web server)

sudo systemctl start httpd  (Start the web server)

sudo systemctl enable httpd (Configure the web server to start with each system)

check the Web Server using the Public IP

sudo usermod -a -G apache ec2-user  (Add the ec2-user user to the apache group)

sudo chown -R ec2-user:apache /var/www  (change the group ownership of the /var/www directory)

Connect your Apache web server to your DB instance
cd /var/www
mkdir inc
cd inc
Create a new file :  >dbinfo.inc
Edit the file:      nano dbinfo.inc

In the dbinfo.inc
Copy the Below code:

<?php

define('DB_SERVER', 'db_instance_endpoint');
define('DB_USERNAME', 'admin');
define('DB_PASSWORD', 'master password');
define('DB_DATABASE', 'sample');

?>


After that we have to create a php file which contain the Php code a particular Directory I will upload the code : Project_PHP_Code.txt for Reference.

Change the directory: cd /var/www/html
Create a PHP file: >SamplePage.php
Edit PHP File: nano SamplePage.php

Verify the Webpage : http://EC2 Public IP/SamplePage.php

After that Connect to the Database to see the Entries.






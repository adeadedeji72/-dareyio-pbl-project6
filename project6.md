# **WEB SOLUTION WITH WORDPRESS, DISTRIBUTED SERVER ARCHITECTURE** #

This project task is to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. 
WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

### **TASK ONE** ###
Launch two Red Hat Linux EC2 instances

![image](red-hat-ec2.jpg)

Update and upgrade the two servers
~~~
sudo yum update -y && sudo yum upgrade -y
~~~

### **TASK TWO** ###
Create and attach three EBS (**ELASTIC BLOCK STORE**) to each of the servers. These will be used to create LVs (**LOGICAL VOLUMES**) on the servers.
Create EBS
Ensure the volumes and your EC2 instances are on the **same availability zone**.

![image](ebs-vol.jpg)

Attach the volumes to each server, three per server.

To attach a volume to a server
- [] Select the volume to be attached on the Volumes page
- [] Click on Action tab
- [] Click on Attach volume
- [] Select the instance you want to attach the volume to
- [] Click on Attach



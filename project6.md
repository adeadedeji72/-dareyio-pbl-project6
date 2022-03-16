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
- [x] Select the volume to be attached on the Volumes page
- [x] Click on Action tab
- [x] Click on Attach volume
- [x] Select the instance you want to attach the volume to
- [x] Click on Attach

Confirm the attached volumes with
~~~
sudo lsblk
~~~
![](lsblk-initial.jpg)

Create a partition on each of the volume attached 
~~~
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
~~~

Check again
~~~
sudo lsblk
~~~
![](lsblk-second.jpg)

Install *lvm2*
~~~
sudo yum install lvm2
~~~
Run lvmdiskscan to check available partitions
~~~
sudo lvmdiskscan
~~~
Use pvcreate to mark each of 3 disks as physical volumes (PVs) to be used by LVM
~~~
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1
~~~
Confirm the physical volumes
~~~
sudo pvs
~~~

Use *vgcreate* utility to add all 3 PVs to a volume group (VG). Name the VG **webdata-vg**
~~~
sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
~~~
Check the volume groups
~~~
sudo vgs
~~~
![](gvs.jpg)

Use *lvcreate* utility to create 2 logical volumes. **apps-lv** (Use half of the PV size), and **logs-lv** Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs
~~~
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
~~~

Confirm the *Logical Volumes*
~~~
sudo lvs
~~~

![image](lvs.jpg)

Verify all to view view complete setup - VG, PV, and LV
~~~
sudo vgdisplay -v
sudo lsblk
~~~
![](view-all.jpg)

Use *mkfs.ext4* to format the logical volumes with *ext4* filesystem
~~~
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
~~~

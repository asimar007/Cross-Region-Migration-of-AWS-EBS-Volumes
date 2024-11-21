

# Cross Region Migration of AWS EBS Volumes

### Project Description

* This project involves migrating AWS Elastic Block Store (EBS) volumes from one region to another. The goal is to seamlessly transfer data and configurations, ensuring minimal downtime and maintaining data integrity. The process includes planning, snapshot creation, volume replication, and final verification to successfully relocate EBS volumes across AWS regions.

### Architecture

![Comming Soon](https://github.com/asimar007/Cross-Region-Migration-of-AWS-EBS-Volumes/blob/main/Screenshot/Cross-Region.jpg?raw=true)

### Elastic Block Store (EBS) 
EBS is a scalable and high-performance block storage service that provides persistent storage volumes for EC2 instances. It allows users to create and attach storage volumes to EC2 instances, offering reliable and low-latency storage for applications and data in the AWS cloud.

### ## Step 1: Launch an EC2 Instance in the N. Virginia Region
-   Log in to your AWS Management Console.
-   Navigate to the EC2 Dashboard and click “Launch Instance.”
-   Choose an Amazon Machine Image (AMI), I have selected Amazon Linux and configure the instance.
-   Launch the instance and confirm it’s running.
-   Here you can see our instance is running on  **“us-east-1b”** availability zone.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/1.png?raw=true)

## **Step 2: Create an EBS Volume in the N. Virginia Region**

-   Click on Create volume

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/2.png?raw=true)
-   Here in the Size you can give according your requirements, I have given 1GiB
-   And in the Availability Zone make sure to select those availability zone where we have launched our instance.
-   Here you can give the Tags and click on Create volume.
![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/3.png?raw=true)
-   Now select your instance and click on connect you can use other tools like GitBash, putty for login.
-   login as root user by  
```
sudo su - root 
```  
```
fdisk -l
```  
-  The command is used to list the partitions and their details on a Linux system.
-   here you can see only one disk is attached.
![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/4.png?raw=true)
-   Now let’s attach the EBS volume, select the volume that we have created, click on action and click on Attach volume.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/5.png?raw=true)

-   Here select the instance that we have launched and click on Attach volume.
-   Connect the instance and see new disk is attached.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/6.png?raw=true)

## Step 3: Store Data in the EBS Volume

-   for storing the data in the ebs volume we have to create a partition.
-   To create a partition we have to go inside the hard disk with  command.
```
fdisk /dev/xvdf
```

- Type n for new partition 
-  type p for primary partition 
-  partition number 1
- First sector enter
- command to specify the Size  **+100M**

- Type w this command will save the partition

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/7.png?raw=true)

The `lsblk` command in Linux is used to list information about block devices, Here you can see one partitons is created.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/8.png?raw=true)

Now we have to **format** the partition for this `mkfs.ext4` command in Linux is used to create an ext4 file system.
```
mkfs.ext4 /dev/xvdf1
```
![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/9.png?raw=true)

-   now create a folder named myfold by  `mkdir /Asim`  command.
-   now we have to mount this folder to the created partition by   command.
```
mount /dev/xvdf1 /Asim
```
-   now we see the the list of mount folder by  `df -h`  command, here you can see our folder is mounted.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/10.png?raw=true)

-   now go inside this folder by  `cd /Asim`  command and create a file and store the data.

## Step 4: Create a Snapshot of the EBS Volume

-   for creating a snapshot select the ebs volume and click on action and click Create snapshot.
![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/11.png?raw=true)

-   Give the tags and click on create snapshot.

## Step 5: Copy Snapshot to the Mumbai Region

-   Now copy this snapshot in which region you want, I am copying to the Mumbai Region, for this select snapshot and click on action and click on Copy snapshot.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/12.png?raw=true)

-   Select the Destination Region in my case it’s  **ap-south-1** and click on copy snapshot .
![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/13.png?raw=true)

## Step 6: Create an EC2 Instance in the Mumbai Region

-   Navigate to the EC2 Dashboard in the Mumbai region.
-   Click “Launch Instance.”
-   Choose an AMI and configure the instance in Mumbai.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/14.png?raw=true)

## Step 7: Create EBS volume from snapshot and attach to the instance.

-   In the Ohio region, go to ec3 dashboard, where click on Snapshot here you can see the one snapshot is copied from N.Virginia region.
-   Now select this snapshot and click on actions and click on Create volume from snapshot.
![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/15.png?raw=true)
-   Here you can define the size of volume how many size you want according you can give, I have just given 1GiB.
-   And select Availability Zone where our instance is running in the Mumbai region in my case it’s running in  **ap-south-1b** AZ and click on create volume.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/16.png?raw=true)
-   Now login to the console switch to root user and check the list of disk here you can see only one disk is attached.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/17.png?raw=true)
-   Now attach the volume to the instance, select the volume and click on actions and click on Attach volume.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/18.png?raw=true)

-   Here select the instance which we have launched and click on attach volume.

![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/19.png?raw=true)

-   now check the list of disk by  command, here you can see one more disk is come up which size is 1GiB.
```
fdisk -l
```

-   Now we have to format this partition by  command.
```
mkfs.ext4 /dev/xvdf1
```
-   Now create a folder by  command here myfold is just a name of folder you can give anything.
```
mkdir /Asim
```
-   Now mount this folder to the new disk by  command.
```
mount /dev/xvdf1 /Asim
```
## Step 8: Verify Data Integrity in Mumbai

-   Now go inside this folder by  `cd /Asim`  command and list the file by  `ls`  command here you can see the file which we have create in N.Virginia region.
-   Now see the data which we have stored by  `cat aws.txt`  command, here you can see the exact same data.


![Alt text](https://github.com/asimar007/Moving-AWS-EBS-Volumes-from-one-Region-to-Another-Regions/blob/main/Screenshot/21.png?raw=true)

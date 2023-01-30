# project-6
WEB SOLUTION WITH WORDPRESS

Project 6 consists of two parts:


1: Configure storage subsystem for Web and Database servers based on Linux OS. The focus of this part is to give you practical experience of working with disks, partitions and volumes in Linux.

2: Install WordPress and connect it to a remote MySQL database server. This part of the project will solidify your skills of deploying Web and DB tiers of Web solution.

So as a DevOps engineer, our deep understanding of core components of web solutions and ability to troubleshoot them will play essential role in our further progress and development.

Three-tier Architecture
Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.

As shown in the diagram below

<img width="773" alt="Screenshot 2023-01-30 at 03 11 13" src="https://user-images.githubusercontent.com/118350020/215373383-6548b36a-beeb-4374-b500-8a014651359b.png">

1: Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.
2: Business Layer (BL): This is the backend program that implements business logic. Application or Webserver
3: Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server

In this project, you will have the hands-on experience that showcases Three-tier Architecture while also ensuring that the disks used to store files on the Linux servers are adequately partitioned and managed through programs such as gdisk and LVM respectively.

Note: We are gradually introducing new AWS elements into our solutions, but do not be worried if you do not fully understand AWS Cloud Services yet, there are Cloud focused projects ahead where we will get into deep details of various Cloud concepts and technologies – not only AWS, but other Cloud Service Providers as well.

Your 3-Tier Setup
A Laptop or PC to serve as a client
An EC2 Linux Server as a web server (This is where you will install WordPress)
An EC2 Linux server as a database (DB) server

Use RedHat OS for this project

Note: for Ubuntu server, when connecting to it via SSH/Putty or any other tool, we used ubuntu user, but for RedHat you will need to use ec2-user user. Connection string will look like ec2-user@<Public-IP>

So lets get down to the real deal now.

Now, we are going to launch EC2 instance that will serve as webserver 

Step 1 — Prepare a Web Server
Launch an EC2 instance that will serve as "Web Server". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

<img width="944" alt="Screenshot 2023-01-30 at 05 01 18" src="https://user-images.githubusercontent.com/118350020/215385024-f2acbb85-7356-4990-abdc-ccfeed097fb7.png">

 Attach all three volumes one by one to your Web Server EC2 instance

<img width="960" alt="Screenshot 2023-01-30 at 05 05 37" src="https://user-images.githubusercontent.com/118350020/215385354-088368d7-1507-4d32-9a4f-1ffceb6924ca.png">

2: Open up the Linux terminal to begin configuration
3: Use lsblk command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. 

<img width="931" alt="Screenshot 2023-01-30 at 06 17 29" src="https://user-images.githubusercontent.com/118350020/215393605-88c1cc33-54c5-4ce8-a484-2767c7b937a9.png">

Inspect it with ls /dev/ and make sure you see all 3 newly created block devices there – their names will likely be xvdf, xvdh, xvdg.


<img width="470" alt="Screenshot 2023-01-30 at 06 35 44" src="https://user-images.githubusercontent.com/118350020/215395827-d7f1e5df-fdaf-4b61-93ab-0d1c39b8c1cc.png">

4: Use df -h command to see all mounts and free space on your server

<img width="418" alt="Screenshot 2023-01-30 at 06 39 47" src="https://user-images.githubusercontent.com/118350020/215396295-4163523d-aa33-4bc0-a5b5-ebff566bad7e.png">

5: Use gdisk utility to create a single partition on each of the 3 disks, so to get this done,we are going to use the command below

sudo gdisk /dev/xvdf

"><img width="558" alt="Screenshot 2023-01-30 at 06 50 58" src="https://user-images.githubusercontent.com/118350020/215397796-4c829c9f-9c1e-464d-883a-35d2eb0ed92b.png">

<img width="441" alt="Screenshot 2023-01-30 at 08 15 46" src="https://user-images.githubusercontent.com/118350020/215411743-0230f965-3d6c-418d-9966-fd50829634c2.png">

so by default , we are going to select 1

<img width="449" alt="Screenshot 2023-01-30 at 08 17 46" src="https://user-images.githubusercontent.com/118350020/215412107-e891b18a-a9ad-44ec-badd-3796d73142ea.png">

if am going to use the entire GB i will just click enter

<img width="488" alt="Screenshot 2023-01-30 at 08 21 17" src="https://user-images.githubusercontent.com/118350020/215412718-b739f539-6a13-4761-a7f7-909ef473e058.png">

so i will changed the file type to 8e00

<img width="488" alt="Screenshot 2023-01-30 at 08 21 17" src="https://user-images.githubusercontent.com/118350020/215414200-e543910f-9ef5-403a-a5cd-9bbddb92e39c.png">
so am going to use command p to check what i have done

<img width="677" alt="Screenshot 2023-01-30 at 08 31 45" src="https://user-images.githubusercontent.com/118350020/215414662-64221c7c-bfc2-4afa-ba1d-d16a54a99889.png">

this shows that all have done is correct

so we are going to use w to write, after that i will say yes

<img width="715" alt="Screenshot 2023-01-30 at 08 38 50" src="https://user-images.githubusercontent.com/118350020/215416138-1af121d1-96c2-4199-bbcc-84d22289a69d.png">

Now,  your changes has been configured succesfuly, exit out of the gdisk console and do the same for the remaining disks.

So the next disk is xvdg
we are going to repeat the same step for the remaining 2 disk

<img width="926" alt="Screenshot 2023-01-30 at 08 49 02" src="https://user-images.githubusercontent.com/118350020/215418265-07d03dc5-9f5d-45de-8409-584caf5f9e36.png">

same step will be taking for the 3 disk
So the next disk is xvdh

<img width="617" alt="Screenshot 2023-01-30 at 09 01 13" src="https://user-images.githubusercontent.com/118350020/215421329-709f2fd9-72fb-4d82-9ecc-8e3773a68f29.png">

so we are going to use lsblk utility to view the newly configured partition on each of the 3 disks.

<img width="464" alt="Screenshot 2023-01-30 at 09 09 11" src="https://user-images.githubusercontent.com/118350020/215422091-07028a6d-9211-4f82-8f30-4238bade1f39.png">

so we are going to Install lvm2 package using the below command
sudo yum install lvm2 -y

<img width="1440" alt="Screenshot 2023-01-30 at 09 19 16" src="https://user-images.githubusercontent.com/118350020/215424114-c7c42162-3293-4599-8b07-9913cbbafd95.png">

<img width="1440" alt="Screenshot 2023-01-30 at 09 20 27" src="https://user-images.githubusercontent.com/118350020/215424222-ee388a30-ec48-43fe-952e-faad1becf889.png">


to check if it as being instslled, we are going to use this command
which lvm

<img width="710" alt="Screenshot 2023-01-30 at 09 24 24" src="https://user-images.githubusercontent.com/118350020/215425443-21927827-35c6-4f73-a8e2-ec240f43c7ed.png">

the next next thing to do now is to create a physical volume

Note: Previously, in Ubuntu we used apt command to install packages 
but in RedHat/CentOS a different package manager is used, so we shall use yum command instead.

Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

so am going to use this commands for the 3 disk  seperately 
sudo pvcreate /dev/xvdf1
sudo pvcreate /dev/xvdg1
sudo pvcreate /dev/xvdh1

note i can as well create it once, so i will uuse this command to create it once
sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1

<img width="581" alt="Screenshot 2023-01-30 at 09 38 21" src="https://user-images.githubusercontent.com/118350020/215427890-bd8cc1b7-f2a5-47f0-bc72-2911b118f0a4.png">

as you can see from the diagram, the physical as being created.

if we run sudo pvs, you will see it clearly

<img width="558" alt="Screenshot 2023-01-30 at 09 41 32" src="https://user-images.githubusercontent.com/118350020/215428588-305f7a03-9757-4b43-8037-df91d01bcee3.png">

so we are going to Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
using the below command below

sudo vgcreate vg-webdata /dev/xvdf1 /dev/xvdg1 /dev/xvdh1

<img width="639" alt="Screenshot 2023-01-30 at 10 03 51" src="https://user-images.githubusercontent.com/118350020/215433650-d65e12fa-0229-40de-8971-753be0d1dde4.png">

we need to verify if our VG has been created successfully by running sudo vgs

<img width="629" alt="Screenshot 2023-01-30 at 10 08 00" src="https://user-images.githubusercontent.com/118350020/215434332-b9b623e5-f93c-4f4e-9f94-0f36894f6067.png">

so now, we are going to use. lvcreate utility to create 2 logical volumes. 
apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. 
NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.

so we are going to use this commmand below
 
 sudo lvcreate -n apps-lv -L 14G vg-webdata
 
<img width="793" alt="Screenshot 2023-01-30 at 10 30 09" src="https://user-images.githubusercontent.com/118350020/215439554-510a67a2-94a3-4330-a992-5d6ec464fe6e.png">

 sudo lvcreate -n logs-lv -L 14G vg-webdata
 
 <img width="662" alt="Screenshot 2023-01-30 at 10 35 20" src="https://user-images.githubusercontent.com/118350020/215440588-7cdecfd8-254b-4fa9-bbe0-052cf3de3c84.png">
 
 to verify that our Logical Volume has been created successfully we  are going to run
 
 sudo lvs
 
 <img width="719" alt="Screenshot 2023-01-30 at 10 46 25" src="https://user-images.githubusercontent.com/118350020/215443369-31103d87-d371-4160-9324-7c8ff42d1478.png"> 
 
 now we are going to Verify the entire setup, so we are using the below command
 
 sudo lvs, sudo vgs and sudo pvs 
 
 <img width="657" alt="Screenshot 2023-01-30 at 10 59 44" src="https://user-images.githubusercontent.com/118350020/215573300-ad22de29-2973-4891-ae8b-28441018bae2.png">
 
 The next thing to do now is to Use mkfs.ext4 to format the logical volumes with ext4 filesystem
 so we are going to run the command below
 
 sudo mkfs.ext4 /dev/vg-webdata/apps-lv 
 
 <img width="622" alt="Screenshot 2023-01-30 at 20 26 48" src="https://user-images.githubusercontent.com/118350020/215576126-8e241e50-a0e4-47a9-a50d-1f405b1c3019.png">

this for the first one, so we are going to use the below command to create for logs-lv
 
 sudo mkfs.ext4 /dev/vg-webdata/logs-lv
 
 <img width="650" alt="Screenshot 2023-01-30 at 20 31 08" src="https://user-images.githubusercontent.com/118350020/215577187-16489b29-2e1b-411f-a7af-f91e0244d784.png">

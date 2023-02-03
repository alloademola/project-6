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

 15 :  the next step is to Create /var/www/html directory to store website files
so we are going to use this command

 sudo mkdir -p /var/www/html
 
 <img width="610" alt="Screenshot 2023-01-30 at 21 13 53" src="https://user-images.githubusercontent.com/118350020/215585329-802f33a5-d821-45d7-b90d-61e3e0403329.png"> 

 
 so we are going to Create /home/recovery/logs to store backup of log data, we are running this command below
 
 sudo mkdir -p /home/recovery/logs
 
 so we are going to mounting point now, we are to Mount /var/www/html on apps-lv logical volume 
 
 and we are going to use the below command
 
 sudo mount /dev/vg-webdata/apps-lv /var/www/html
 
 <img width="682" alt="Screenshot 2023-01-30 at 21 27 33" src="https://user-images.githubusercontent.com/118350020/215587851-07e03e1a-af5d-4442-b7be-8828b45ecd25.png">

 from the diagram above,we can see it is mounted
 
 
 to confirmed if my /log is not empty,  we are going to use this command below to run that
 
 sudo ls -l /var/log 

 <img width="667" alt="Screenshot 2023-01-30 at 21 35 33" src="https://user-images.githubusercontent.com/118350020/215589349-e82e2344-9943-4efd-a743-f35f7aa4445d.png">

 
 what next is to Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
 
 so we are to run this command below
 
 sudo rsync -av /var/log/. /home/recovery/logs/
 
<img width="670" alt="Screenshot 2023-01-30 at 21 42 09" src="https://user-images.githubusercontent.com/118350020/215590548-e73945f9-fe38-45dc-81d4-eb81ec8aebd6.png">

as you can see from the above diagram, it works fine
 
so we can go ahead and Mount /var/log on logs-lv logical volume. 
Please Note that ,all the existing data on /var/log will be deleted. That is why step 15 above is very
important
 
 so we are going to use this below command to get that done
 
 sudo mount /dev/vg-webdata/logs-lv /var/log 
 
 <img width="670" alt="Screenshot 2023-01-30 at 21 57 10" src="https://user-images.githubusercontent.com/118350020/215593396-69623b9d-1bd7-43d6-9a4b-490de2aecadb.png"> 
 
 as you can see from the diagram above,there such folder is lost or not found
 
 so its very important that we Restore log files back into /var/log directory 
 so we are going to run the below command now
 
 sudo rsync -av /home/recovery/logs/ /var/log
 
 <img width="648" alt="Screenshot 2023-01-30 at 22 02 28" src="https://user-images.githubusercontent.com/118350020/215594270-d2942dd3-e5f5-4e42-9c64-c5b115ec9bb9.png">

 from the diagram above, we can see that we have copy it back to the directory, but let us run this command to be sure
 
 sudo ls -l /var/log
 
 <img width="597" alt="Screenshot 2023-01-30 at 22 05 46" src="https://user-images.githubusercontent.com/118350020/215595402-8f90225f-83e3-4df1-8330-2781e3dd59ae.png"> 
 
 the diagram shows it as being copied back
 
 so now, we need to Update /etc/fstab file so that the mount configuration will persist after restart of the server.
 
 so we are to run this command below to check the block ID and that is what am going to use to populate the fstab
 
 sudo blkid
 
 
 <img width="1060" alt="Screenshot 2023-02-03 at 10 37 09" src="https://user-images.githubusercontent.com/118350020/216566646-65d73c7d-0454-4f51-aca3-7cba016e0919.png">
 as you can see from the above diagram, we have the block ID, so we can easliy copy out the ext4 file, for apps and logs
 
 /dev/mapper/webdata--vg-apps--lv: UUID="a9632105-647a-4cf4-95da-e060ef873763" TYPE="ext4"

/dev/mapper/webdata--vg-logs--lv: UUID="47f62323-a9ca-4349-a6a4-7fcef017c08e" TYPE="ext4"
 
 so am going to run this command below
 
 sudo vi /etc/fstab
  
 <img width="1063" alt="Screenshot 2023-02-03 at 10 49 49" src="https://user-images.githubusercontent.com/118350020/216568736-a520a3fc-04b0-48de-aaa1-e066960cf628.png">
 
 so am going to copied and paste the blockid inside it as shown below in the diagram
 
 <img width="1066" alt="Screenshot 2023-02-03 at 11 02 35" src="https://user-images.githubusercontent.com/118350020/216571900-5ad554c1-6c28-4f59-bc8c-7dc51ac0bca7.png"> 
 
so am going to run this commad below to mount i, and if it returns error, it means what we have done is wrong 
 sudo mount -a
 
 <img width="1063" alt="Screenshot 2023-02-03 at 11 10 51" src="https://user-images.githubusercontent.com/118350020/216573392-bd2be2b1-c794-4b87-ad61-6f393284c086.png">
 
 as you can see from the above diagram, it works.
 so we need to restart it with this command
 
 sudo systemctl daemon-reload
 
 <img width="1105" alt="Screenshot 2023-02-03 at 11 17 10" src="https://user-images.githubusercontent.com/118350020/216574924-98697e5c-92e3-45be-a985-48833295101f.png">
 
 so we have uupdated the fstab as we can see from the diagram above.
 
 so let us progress to the next stage
 
 
 
 
 Step 2 — Prepare the Database Server

 And Launch a second RedHat EC2 instance that will have a role – ‘DB Server’
Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

 first just run this command below
 
 lsblk
 
 <img width="952" alt="Screenshot 2023-01-31 at 23 17 36" src="https://user-images.githubusercontent.com/118350020/215896520-af1e5589-5e83-4341-91c8-f8be44a95c64.png"> 
 
 so we are going to run this command below again
 
 sudo gdisk /dev/xvdf
 
 <img width="939" alt="Screenshot 2023-01-31 at 23 24 49" src="https://user-images.githubusercontent.com/118350020/215897699-ba41cde7-726f-4d1a-a5bf-81fc38b80334.png"> 
 
 <img width="980" alt="Screenshot 2023-01-31 at 23 27 28" src="https://user-images.githubusercontent.com/118350020/215898139-18a67498-0086-4f8e-9f9e-8f5a3ee11a65.png">
 
 we are going to repeat the same step for the remaining xvdg and xvdh
 
 <img width="972" alt="Screenshot 2023-01-31 at 23 33 04" src="https://user-images.githubusercontent.com/118350020/215899329-ac353b96-a38d-4660-97f6-5267b154ae96.png">
 
<img width="978" alt="Screenshot 2023-01-31 at 23 36 55" src="https://user-images.githubusercontent.com/118350020/215899550-14954430-7805-4235-ba23-d7f333a288b3.png">
 
 so we need to check if what we just did now is correct.
 so we are going to run this command below
 lsblk
 
 <img width="978" alt="Screenshot 2023-01-31 at 23 41 06" src="https://user-images.githubusercontent.com/118350020/215900448-b9bb357b-c4a0-44f1-84ea-b59fd26f0067.png">
 as you can see from the diagram ,our configuration is correct

 The next thing now, is to install lvm2, so we are going to run the command below
 
 sudo yum install lvm2 -y
 
 <img width="985" alt="Screenshot 2023-01-31 at 23 54 51" src="https://user-images.githubusercontent.com/118350020/215902542-94f2c133-1fc7-4a4e-b35d-2a2583d8fb18.png">
 
 <img width="983" alt="Screenshot 2023-01-31 at 23 55 47" src="https://user-images.githubusercontent.com/118350020/215902716-6ec16987-cd41-400f-9d8b-50c4cbd483e8.png"> 
  
 so am going to run the below cammand now.
 
 sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1  
 
 <img width="984" alt="Screenshot 2023-02-01 at 00 02 48" src="https://user-images.githubusercontent.com/118350020/215903811-09926c9c-8613-4e7d-a688-77033b5d30b8.png">
 
 next, am going to run this command below again.
 
 sudo vgcreate database-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1 
 
 <img width="979" alt="Screenshot 2023-02-01 at 00 23 19" src="https://user-images.githubusercontent.com/118350020/215906695-8bbaa2b0-bc6a-4f36-b2f4-bbd90e9083da.png">
 
 To. confirmed what we just created, let us run the command below
 
 sudo vgs
 
 <img width="977" alt="Screenshot 2023-02-01 at 00 25 42" src="https://user-images.githubusercontent.com/118350020/215907150-0110af85-e5bc-4ea8-9d0a-d350b7b63fcc.png">
 
 so now , let us created our logical volume, so we are going to create our db-lv only 
 
 we are using the command below and this is the only logical volume we are to created 
 
 sudo lvcreate -n db-lv -L 20G database-vg 
 
 <img width="984" alt="Screenshot 2023-02-01 at 00 36 02" src="https://user-images.githubusercontent.com/118350020/215908230-b3e8ae23-40e2-4130-8674-bcf6d788991e.png"> 
 
 so to confirmed what we just did on that diagram above, we are going to run the command below
 
 sudo lvs
 
 <img width="979" alt="Screenshot 2023-02-01 at 00 39 33" src="https://user-images.githubusercontent.com/118350020/215908673-9d165aaa-3374-4137-bb9e-2e53d6366915.png"> 
 
So the next step is to create the mount point.
 
so we are going to run this command below
 
 sudo mkdir /db 
 sudo mkfs.ext4 /dev/database-vg/db-lv
 
 <img width="980" alt="Screenshot 2023-02-01 at 00 53 38" src="https://user-images.githubusercontent.com/118350020/215910490-bb9efc03-6fcf-4134-a8a0-f9b53fe84c25.png"> 
 

 
 our disgram above shows that, we are doing the right thing
 
 so now we are going to mount,
 
 we are going to use the command below
 
 sudo mount /dev/database-vg/db-lv /db 
 
 <img width="972" alt="Screenshot 2023-02-01 at 01 00 12" src="https://user-images.githubusercontent.com/118350020/215911835-8fe140d3-1137-4374-866d-401e1d61400b.png">
 
 from the diagram above, its shows that, it as being mounted
 
 one last step, we need to put it into fstab
 
 so lets run this command below
 
 sudo blkid 
 
 <img width="1063" alt="Screenshot 2023-02-03 at 11 57 21" src="https://user-images.githubusercontent.com/118350020/216586729-7feb5c9c-238a-4745-8871-c72221db8103.png">
 
 so now let us run this command
 
sudo vi /etc/fstab
 
<img width="1062" alt="Screenshot 2023-02-03 at 12 00 57" src="https://user-images.githubusercontent.com/118350020/216587426-9b433a34-3c08-4eaf-a703-fe1fba22406b.png">
 
 
 so we are going to write this UUID i copy from the blikd inside it
 
UUID=ed07a0b5-36c2-480f-b831-20b1e447791c /db ext4 defaults 0 0 
 
 <img width="1058" alt="Screenshot 2023-02-03 at 12 07 16" src="https://user-images.githubusercontent.com/118350020/216589069-646ff51d-694b-46d1-b551-448fe5bdc826.png"> 
 
 so we have to run this command now
 sudo mount -a and if it returns no error, it shows we are good
 
 <img width="1061" alt="Screenshot 2023-02-03 at 12 23 28" src="https://user-images.githubusercontent.com/118350020/216592123-edd4bd99-9481-41ea-954c-b85996e70702.png">
 
 so as shown above, it returns no error,
 so we are to restart it now with this command below
 
 sudo systemctl daemon-reload
 
So now let us Verify our setup by running df -h, output must look like what is in the diagram below
 
 
 <img width="1059" alt="Screenshot 2023-02-03 at 12 30 05" src="https://user-images.githubusercontent.com/118350020/216592827-c3d474ad-6ffe-482f-a66f-c7b89f9735d0.png">
 
 
 
 
 Step 3 — 
 
 we are to Install WordPress on our Web Server EC2
and Update the repository using the below commands to get that done
 

sudo yum -y update
 
 so after the update we need to

Install wget, Apache and it’s dependencies using the below commands. 

sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
 
 <img width="1058" alt="Screenshot 2023-02-03 at 13 06 08" src="https://user-images.githubusercontent.com/118350020/216600107-19fbb226-6327-4e34-83d0-1163b495e837.png">
 
 <img width="1066" alt="Screenshot 2023-02-03 at 13 04 59" src="https://user-images.githubusercontent.com/118350020/216600190-6b2309ed-4254-47e3-bd67-44fcbf536311.png">
 
  
next step is to install To install PHP and it’s depemdencies 
 
 so we need to run all this command below
 
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
 
 lets check if the php is active, so we are going to run this command
 
 sudo systemctl status php-fpm 
 
 <img width="1053" alt="Screenshot 2023-02-03 at 13 26 25" src="https://user-images.githubusercontent.com/118350020/216603548-a27602ee-0427-4b39-83fb-f39a50a73b41.png">

 
 so to instruct SeLinux to allow Apache to execuute the PHP code via PHP-FPM to run
 we need to run this command 
 
 sudo setsebool -P httpd_execmem 1
  after this is done ,  we need to restart the Restart Apache using the below cammand
 But before that, let us check the status 
 
 let us run this command
 sudo systemctl status httpd
 
 <img width="1046" alt="Screenshot 2023-02-03 at 14 03 31" src="https://user-images.githubusercontent.com/118350020/216610438-11938f3f-1560-4029-9855-b5c31180ff54.png">
 
 now lets get the public IP and run it on the browser to see if the Apache will work
 
 <img width="1440" alt="Screenshot 2023-02-03 at 14 08 54" src="https://user-images.githubusercontent.com/118350020/216611360-3e432738-4d7e-48e6-a271-09687d59d372.png">
 
 <img width="1440" alt="Screenshot 2023-02-03 at 14 09 06" src="https://user-images.githubusercontent.com/118350020/216611405-28ca9c9e-6132-4317-ba4f-662134cbf2dd.png">
 
 as shown in the above diagram, it means that the Apache is working on fine
 
 
 The next step now is to Download wordpress and copy wordpress to var/www/html
 
 we are going to run this command below
 
  mkdir wordpress
  cd   wordpress
  sudo wget http://wordpress.org/latest.tar.gz
 
<img width="1109" alt="Screenshot 2023-02-03 at 14 18 01" src="https://user-images.githubusercontent.com/118350020/216613485-61c1965d-64f9-4f70-a542-ed3b17ac5e6f.png">
  so let us run this command below to check for the latest tar
 
 ls -l
 
 <img width="1104" alt="Screenshot 2023-02-03 at 14 24 53" src="https://user-images.githubusercontent.com/118350020/216614563-96c1a972-f4e6-4e67-9a74-1e78bc825b13.png">
 
 so we have the latest, now let us extract it,we are going to use the below command
 
 sudo tar xzvf latest.tar.gz
 
 <img width="1063" alt="Screenshot 2023-02-03 at 14 27 45" src="https://user-images.githubusercontent.com/118350020/216615292-413fde8e-e7c6-4846-8d7b-bb5bcd23ca47.png">
 
 <img width="1058" alt="Screenshot 2023-02-03 at 14 28 22" src="https://user-images.githubusercontent.com/118350020/216615373-98b67623-c91a-4314-a85d-8afb46579b32.png">
 
 as you can see from the diagram above, it as being extracted
 so another folder have being created called wordpress as you can see from the below diagram 
 
 <img width="1066" alt="Screenshot 2023-02-03 at 14 31 26" src="https://user-images.githubusercontent.com/118350020/216615890-8a15c414-eba6-42d1-8ea3-73145f2e06d0.png">
 
 so am going to change direction into that wordpress, using this command
 
 cd wordpress
 
 <img width="1054" alt="Screenshot 2023-02-03 at 14 33 41" src="https://user-images.githubusercontent.com/118350020/216616288-f442e0a0-bebb-4294-8886-b30ed1f9eb34.png"> 
 
 am going to run a command now to copy from one folder to another using the below command
 
 sudo cp -R wp-config-sample.php wp-config.php
 
 <img width="999" alt="Screenshot 2023-02-03 at 14 46 23" src="https://user-images.githubusercontent.com/118350020/216618833-cd7f37e4-073c-48ab-9d44-67868c8a9bd5.png">
 
 dont forget, am inside wordpress, wordpress inside wordpress, as show in the diagram below
 
 <img width="999" alt="Screenshot 2023-02-03 at 14 48 59" src="https://user-images.githubusercontent.com/118350020/216619570-682b60bd-43a2-4821-8013-7496200a89ce.png"> 
 so i will just run this command now 

 cd..
 ls
 
 <img width="991" alt="Screenshot 2023-02-03 at 14 52 07" src="https://user-images.githubusercontent.com/118350020/216620091-99774c29-90f8-4268-a03a-f2c627930a99.png">
 
 so i will run this command below to copy the content from the wordpress
 
sudo cp -R wordpress/ /var/www/html/ 
cd /var/www/html/ 
 
 <img width="978" alt="Screenshot 2023-02-03 at 15 02 42" src="https://user-images.githubusercontent.com/118350020/216623079-53537e51-7eb6-413d-8225-cd6b220599a3.png"> 
 
 cd ../..
 
 <img width="995" alt="Screenshot 2023-02-03 at 15 12 06" src="https://user-images.githubusercontent.com/118350020/216624998-3ac5d503-3648-43c0-bc3a-303ba4e11487.png"> 
 
 so will run this command below
 ls -l wordpress
 
 <img width="995" alt="Screenshot 2023-02-03 at 15 12 06" src="https://user-images.githubusercontent.com/118350020/216625870-674f9e7a-6df0-439d-8f4d-9d4d506aa4e8.png">
 
 sudo cp -R wordpress/.  /var/www/html/
 to see if it works, i will run the below command as well
 
 sudo ls -l /var/www/html
 
<img width="1001" alt="Screenshot 2023-02-03 at 15 24 12" src="https://user-images.githubusercontent.com/118350020/216627540-4f10374b-da91-49b5-b133-49cf42d84b60.png">
 
 wow, it works fine
 
so lets change directory 
 cd /var/www/html
 
 <img width="998" alt="Screenshot 2023-02-03 at 15 36 14" src="https://user-images.githubusercontent.com/118350020/216630232-41b90eda-536b-4e07-912e-1704c480e234.png">
 
 dont forget that our webserver will be
 acting as a client, so we also need to install mysql inside it as well
 
 so we are to run this command now
 
 sudo yum install mysql-server 
 
 <img width="1003" alt="Screenshot 2023-02-03 at 15 48 08" src="https://user-images.githubusercontent.com/118350020/216633223-de2d66f0-8f79-41e3-9953-634eda35af07.png">
 
 <img width="997" alt="Screenshot 2023-02-03 at 15 48 50" src="https://user-images.githubusercontent.com/118350020/216633275-a005572b-eb6b-41c5-a278-4a622b680e71.png">
 
 so we need to keep our webserver running, so we just have to start it with this command below
 
 sudo systemctl start mysqld 
 sudo systemctl enable mysqld 
 sudo systemctl status mysqld 
 
 <img width="998" alt="Screenshot 2023-02-03 at 15 55 05" src="https://user-images.githubusercontent.com/118350020/216634530-e44e8569-071e-4520-9438-1e5f8fb17cdd.png">
 its all working fine as you can see from the above diagram, its operational 
 
 
 Next Step now is to 
 
 Install MySQL on our DB Server EC2 
 
 so we are to run this command now
 
 sudo yum install mysql-server -y
 
 <img width="1002" alt="Screenshot 2023-02-03 at 16 01 34" src="https://user-images.githubusercontent.com/118350020/216636196-7b0bdd10-267c-4bab-96f9-8bf6cbb12961.png"> 
 
 <img width="995" alt="Screenshot 2023-02-03 at 16 02 34" src="https://user-images.githubusercontent.com/118350020/216636374-fb3f65c5-f28a-46c8-8af6-79fa2d642991.png">

 now let us run this command below to make the server run
 
 sudo systemctl start mysqld
 sudo systemctl enable mysqld 
 sudo systemctl status mysqld  
 
 <img width="989" alt="Screenshot 2023-02-03 at 16 07 27" src="https://user-images.githubusercontent.com/118350020/216637492-6b7a66be-14ad-4b44-b6d7-36383ec65eba.png">
 
 from the diagram above, our server is running
 
 so now lets run this command below
 
 sudo mysql_secure_installation 
 
 <img width="1004" alt="Screenshot 2023-02-03 at 16 11 35" src="https://user-images.githubusercontent.com/118350020/216638372-7c2242fd-eea4-40e0-8942-eb0f2ae32823.png"> 
from the Diagram above, we are just going to say n
 and i will just give a simple password
 
 so just continue to type yes till the end, as shown in the diagram below
 
 <img width="999" alt="Screenshot 2023-02-03 at 16 16 28" src="https://user-images.githubusercontent.com/118350020/216639488-e54e7687-0b2c-463b-ad3a-8a608755a80d.png">
 
 let us run this command sudo mysql -u root -p
 
 <img width="998" alt="Screenshot 2023-02-03 at 16 19 30" src="https://user-images.githubusercontent.com/118350020/216640176-e5ba56f4-5300-4306-a727-bed510a92383.png">
 
 now input the password you set while installing the mysql as you can see below from the diagram
 
 <img width="992" alt="Screenshot 2023-02-03 at 16 20 01" src="https://user-images.githubusercontent.com/118350020/216640406-8cd1cbaa-f06a-4dfa-9820-ef9799c29c32.png"> 
  so now let us create our database using the below code
 
 create database wordpress;
 
 <img width="1010" alt="Screenshot 2023-02-03 at 16 25 35" src="https://user-images.githubusercontent.com/118350020/216641485-c1fa8841-5632-4688-9214-8a648bb890e5.png">
 
 so if i enter show databases; it should give us whats inside the below diagram
 <img width="1440" alt="Screenshot 2023-02-03 at 16 29 44" src="https://user-images.githubusercontent.com/118350020/216642472-916b55b1-6a3d-47ee-9fb0-0efa2034f483.png">
 
 <img width="796" alt="Screenshot 2023-02-03 at 16 49 50" src="https://user-images.githubusercontent.com/118350020/216647178-2db95554-56cc-497c-a0c2-6b24b5c6749d.png"> 
 
 next is to FLUSH PRIVILEGES; and 
SHOW DATABASES;
exit
 
 <img width="1000" alt="Screenshot 2023-02-03 at 16 56 05" src="https://user-images.githubusercontent.com/118350020/216648536-997f297d-567d-4aed-8ab2-835db779327a.png">
 
 we also need to set the bind address to, so we are to run this command below

sudo vi /etc/my.cnf 
 
 <img width="999" alt="Screenshot 2023-02-03 at 17 00 31" src="https://user-images.githubusercontent.com/118350020/216649502-6bbde376-d0d9-44be-a008-f970b4c5116d.png">
 
 so we are going to input this code below
[mysqld]
bind-address=0.0.0.0 as shown in the diagram below
 
 <img width="1440" alt="Screenshot 2023-02-03 at 17 05 22" src="https://user-images.githubusercontent.com/118350020/216650591-4faf69a6-f190-4804-bee6-457e3f1fd222.png">
 
 so we need to restart the service using the below command
 
 sudo systemctl restart mysqld 
 
 so now lets go back to our webserver
 
 so we are going to run this command now
 
 sudo vi wp-config.php
 
 <img width="1440" alt="Screenshot 2023-02-03 at 17 11 23" src="https://user-images.githubusercontent.com/118350020/216651820-4b4eb924-94f1-47a7-a6d6-67eeebdcb601.png">
 
 <img width="1440" alt="Screenshot 2023-02-03 at 17 11 30" src="https://user-images.githubusercontent.com/118350020/216651846-cc88123c-5a2e-4f78-8b54-a9add77e57b4.png">
 
 so we are going to edit now
 
 
 
 

**Block Storage**

- Breaks up the data into blocks and then stores these blocks as seperate pieces, each with the unique identifier.
- A collection of blocks can be presented as volume to the operating system.
- We can boot and mount from block storage

**Elastic Block Storage**

- It provides block level storage volumes for use with EC2 instance.
- EBS volume is maintained in an availabiltiy zones
- To copy data from an EBS volume to other AZ, you can create the EBS Snapshot.
- EBS offers different volumes types for the different storage needs.
- We will be charged for what we are provisioned - charge are in GB per month.

**Steps to do after created EBS**

- Once you created the EBS and attached to EC2 means.
- Then you need to ssh to the ec2 instance
- Run **sudo file -s /dev/xvdf**  this command is used to get to know that created EBS volume is filesystem exists or not.
- If it return **data** means  its is raw volume, No filesystem exists yet, like a new hard disk
- Run **lsblk** command to get the details of the attached volume
- run **sudo mkfs -t xfs /dev/xvdf** command to activate it has filesystem.
- create a new folder inside home like ebsdemo
- run **sudo mount /dev/xvdf /home/ec2-user/ebsdemo** to mount directory volume attached.

**TO configure the filesystem for automatic mounting during the boot process, follow below steps**

- ran **sudo blkid** to obtain the UUID of the attached volume
- edit the **etc/fstab** file and change the UUID there.
- Editl ike UUID="xxxxxxxxxxx" /home/ec2-user/ebsdemo xfs defaults nofail
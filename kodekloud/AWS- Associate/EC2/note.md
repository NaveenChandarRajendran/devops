**EC2 Instantce Types**

- General Purpose
- Compute Optmized
- Memeroy Optimized
- Storage Optimized
- GPU Instances

**AMI - Amazon Machine Image**

It is the image of the os. It is blueprint to ec2

- We can choose type of ami on creating the ec2 at mimic the actual os.
- The main advantage is the we are not only going to just create the os, the ami wil have additional softwares like webserver and other essentials for that image.

**Types of AMI**

- Public AMI - (open image to deploy it is free )
- Private AMI - (Only created user will have access to control the ami)
- Shared AMI - (This is also private but we can collabarate by providing the other aws users)

**Custom AMI**

- We also can create the custom AMI. Because we can config what are essential thing we need upon on the aws default software for ami.
- By this we can use in on the every instance in ec2,we dont want to manually create from first.

**SSH**

- On creating the ec2, we also need to configure the SSH.
- By setting the **private key** and **public key.**
- The public key is already present in the instance, we need to access it by sending the private key.

**Ec2 Instance Lifecycle**

![image.png](attachment:26212f9a-5416-4459-bf83-7dbfc7bf761a:image.png)

**User Data**

- On creating ec2 instance we can give the boostrap script so that on creating and first set of work this script will execute, in that we can use if for setup the firewall, adding users and cloning any git project and make it ready.
- It has the max size of 16kb.

**EC2 with Security Group**

- We also need to create the secuirty group.
- WE need to say what traffic is allowed inbound and traffic allowed for outbound.
- Basically we need to config as inbound traffic port on 80, which os for HTTP
- Port 443 which is for HTTPS

**EC2 with EBS - Elastic block storage**

EC2 instances handle the compute aspect things, so that runs our application code for this the EBS comes into play.

- When instance want to store the data and prsistent data, we are going to utilize EBS volumes.
- It has additional functionalities like takes snapshot.
- The snapshot will be stored in the s3 storage. It will take backup at several interval of time.

![image.png](attachment:7a64c187-2900-4064-a273-89265ce4b605:image.png)

**EC2 with ELG and AS - Elastic LoadBalencer and Autoscaling Groups.**

- We can configure the ec2 with ELG and AS as it is aws services
- Traffic comes across the mulitple targets,such as ec3 instances , container or IP Addresss.
- If we deploy like web app in the multiple ec2 instances, the load balancer is going to load balance it across them
- So the end user dont have remember the ip address of each and everyone servers
- They just send a request to load balancer , and that will load balence across the different EC2 Instances within the groups.
- For **AS,** image we have configered an 4 instance and we will got the high traffic like we put some sales or offer or live etx, that time it will **automaticaly scale** and add more EC2 instances to handle the demand.

**EC2 with Elastic IP**

- When you have instance which is shut down and next day you restarted it means the ip address will change.
- That dont want because the end user will get affect due to the ip address changes.
- In that case we can use one of the **aws service is elastic ip.**
- It is the reversed ap address system. the same id will be set to the instance whenever it shutdown and started.
- In future if we created the new instance in that also we can use this elastic ip.

**EC2 launch Template**

- The cloud professional will create the launch template , that means it will have the configuration and step which needs to be follow on creating the instance.
- Ex: In autoscale group it will create the instance automatically how it will know the what instance configure i need to create so if we configure the launch template it will create instance based on the config.

**EC2 instance placements**

- When deploy in ec2 instance , it will be placed in any server within the amazon data centers or an availability zone.
- But we need to control the deploy location like we want ec2 instance placed in a specific location.
- To that we have 3 options. They are
- **Cluster Placement Group -** The idea for this is to place the instance close to one another as possible. This is helpfull for application that need low network latency,high computing apllications , big data and analytics workloads.
- **Partition Placement Group -** If we want tighlty close as possible, then we have partition placement.
    - This is placed as partitions, ensuroing that isntance in one partition does not share the same hardware of others.
    - Also each partions is going to have its own set of racks and each rack going to have its own network and power sources.
- **Spread** **Placement Group -**  IF we fine with the same hardware as instance in another partions we can use this.
    - This spreads out the instances acroess hardware to reduce the high presure for one hardware.
    - This is will be ideal for small number of critical instances
    - Each instance is placed on a distint rack with each rack having its own network and power sources

![image.png](attachment:5f6b8fb6-0804-45ff-9619-0bb777c094dd:image.png)

**EC2 Instance Purchasing option**

![image.png](attachment:3881dea8-b252-4910-af1e-db187df4a500:image.png)

- **On Demand -** The bill will be based on our usage. we can shutdown whenever we not using the instace.
- **Spot -** It has the discount on ec2 rarely we will get upto 90% discount. But it not stable what instance we will get dont know.
- **Saving Plans -** Monthly wise subcribing to an instance, what ever the isntance is running on full day or half day
- **Reserved Instance -** It is same as the saving plan but diffenrent is we are commiting monly or annually for computing power of the instance, If we have more intense or sale for the specific time and other times we will have normal traffic means in such scenario it will work.
- **Dedicated Hosts -** Owning the dedicated host is reserved and all the ec2 instances and because they ar running on the same host,we get to resuse the software licensses that are in the same host.
- **Dedicated Instance -** Owning the server ro deploy all the instance we have
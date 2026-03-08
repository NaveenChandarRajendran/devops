- For every region we have one VPC.
- We have 2 types of VPC.
    - Default VPC
    - Custom VPC

**Default VPC**

- One Default VPC for region
- /16 IPv4 CIDR block 172.31.0.0/16 is given with (66,536) address
- /20 default subnet in each availability zone (4096 address)
- Diagram

![image.png](attachment:21b3bbf0-043e-4bf8-8dce-f5a8ecfd1f6a:image.png)

- **Internet Gateway** attached to the VPC.
    - a route that points all traffic (0.0.0.0/0) to the internet gateway.
    - Within this default subsets will be accessible from the internet
    - Default Security Group
- **Default Security Group**
    - Work with instance level
    - **InBound**
        - Controls WHO can send requests TO your instance (react-app).
        - Example 1: A user from internet opening your website (port 80/443)
        - Example 2: Your Node.js EC2 calling your React/Nginx EC2 internally
        - **Default Rule**: Only allows traffic from same security group.
        Blocks everything else.
    - **OutBound**
        - Controls WHERE your instance (react-app) can send requests TO.
        - Example 1: Your EC2 calling an external API (like Stripe, Google)
        - Example 2: Your EC2 downloading packages (npm install, apt-get)
        - Example 3: Your Nginx EC2 calling Node.js EC2 internally
        - **Default Rule**: Allows ALL outbound traffic (0.0.0.0/0)
- **NACL - Network Access Control List**
    - Works at subnet level
    Like a **security guard at the main gate** of your apartment complex
    - Every request must pass NACL first, then Security Group

- **NACL vs Security Group**
    - NACL = **Subnet gate** | Security Group = **Instance door**
    - NACL = **Stateless** | Security Group = **Stateful**
    - NACL = **Allow + Deny** | Security Group = **Allow only**
    - NACL = **Rule numbers matter** | Security Group = all rules evaluated together

**Stateless (Most Important)**

- NACL does NOT remember previous requests
- Must explicitly allow **both inbound AND outbound** for every communication
- Response comes back on **ephemeral ports (1024–65535)** — must allow these too
- Example: Node.js calls Stripe API
    - Outbound rule needed → port 443 to internet
    - Inbound rule needed → port 1024-65535 for response coming back

### Key Rule to Remember

`NACL  → Stateless → Allow request + Allow response (2 rules needed)
SG    → Stateful  → Allow request only (response auto-allowed)`

**Subnets**

- Subnets are group ip address in our vpc.
- A subnet resides within a single **Availability Zone.**
- Subnets can ne puclic or private to allow external resouse access within them
- Subnets within a VPC must be within CIDR range.
- The First 4 IP address is reserved
    - 192.168.10.0 → Network Address
    - 192.168.10.1 (VPC Router)
    - 192.168.10.2 (DNS)
    - 192.168.10.3 (Future Use)
- The last ip address 192.168.10.255 is reserved as the broadcast address
- **Subnets Configuration options**
    - IT cannot be overlap with other subnet in the VPC.
        
        ![image.png](attachment:ded91dfa-398c-4b5b-9234-93fa1aee4bb6:image.png)
        
    
    → The Subnet can communicate with other subnets within the VPC. 
    
- **Routing VPCs**
    - For Each VPC we have router interface.
    - The purposr router is to route the traffic betweens subnets. as well as in and out of the VPC
    - Just like the data centers ****the aws give configation options to determine kind of route should works
    - If traffic comes it will be works on the destination and target asppects.
        - **Destination -** The IP range you're trying to reach
        - **Target -** The path to use to get there.
    
    ![image.png](attachment:81da19f1-bfdc-4512-a340-e544105d18c8:image.png)
    
- Internet Gateways
    - Enable resource to connect to internet
    - Intenet Gateway attach tp a VPC.
    - VPC can only have 1 internet gateway and internet gatewa y can be attached to max 1 VPC
    - A Subnet default it will be private ip,we need to change the subnet to public manually. and its made publoc once a default route points to the internet gateway in the VPC
    - Ex: In the route table add the extra ip like 0.0.0.0 in **destination** that means anyclient is accessing our instance means accepet and set **target** as local.
    - For above example any client (browser) hit our instance means it will send to internetGateway.

- **NAT Gateway - Network Address Translation**:
    - In above internet gateway we make the private subnet to public so that any one access our instance through intenet.
    - But if you want **only the instance want to talk to internet for any updates but the other open world cant access our site.** that way the NAT Gateway will work.
    - It **TRANSLATES** your private IP → to a public IP
    So internet thinks request is coming from NAT, not your EC2.
    - Ex:
        - Your Node.js is in a **private subnet** — no direct internet access.
        
        But Node.js needs to:
        
        `✅ Call Stripe API          → payment processing
        ✅ Send emails via SendGrid → notifications  
        ✅ Call Twilio              → SMS
        ✅ npm install packages     → updates
        ✅ Call any external API    → business logic`
        
        **Problem:**
        
        `Node.js (10.0.2.15) → tries to call api.stripe.com
        Route Table: 0.0.0.0/0 → ??? 
        No Internet Gateway (private subnet!)
        → ❌ REQUEST FAILS!`
        
        **Solution = NAT Gateway!**
        

Node.js EC2                    NAT Gateway              Stripe API
(10.0.2.15)                   (54.12.34.99)            (54.187.x.x)

```
 │                              │                        │
 │── "Call Stripe for me" ──▶  │                        │
 │                              │── request ──────────▶ │
 │                              │   (from 54.12.34.99)  │
 │                              │◀─ response ────────── │
 │◀─ "Here's the response" ──  │                        │
 │                              │                        │
```

Stripe only sees NAT's IP (54.12.34.99)
Stripe never knows Node.js (10.0.2.15) exists!

- The NAT gateway is depend on the subnet. so it will be deployed on the avaibility zones sepcific not as region sepecfic.
- So we cant configure NAT to one avaiblixity zone.We need to configure to all avaiablity zone then only if one is gone down other will do the work.
- Billing wise is for hour based

- **Elastic IPs**
    - Image we have instance ip adress as 1.1.1.1 but when the instance is rebooted the ip address will be changed. somelthing like 2.2.2.2 that cause issue.
    - Because our consumers will bookmark and know the 1.1.1.1 ip only next day if they ping 1.1.1.1 it will throw error as not know
    - TO control this AWS developer Elastic IPs
    - In Elastic Ip we can get the ipv4 address that doesnt change anytime.
    - So we can attach that elastic ip to our instance so if the instance is rebooting or some changes is happening means we dont care because we the elastic ip address wotn change for the instance.
    - We can move the one elastic ip to other instance so if one instance is goes to mainataince or some work we can shift the elastci id to other instance to continue the flow
    - It is specific to region

- Security Groups & NACLS
    - Stateless Firewalls
        - Firewall rules are broken down into **inbound** and **outbound** rules.
        - Both inbound and outbound needs to always allow in stateless firewalls
    - Statefull Firewalls
        - This is inteligent enought to understand which request and response are part of the same connection.
    
    So One type of Firewall aws provide is NACL- Network Access Control List
    
    - NACL filter traffic enter and leaving a subnet
    - NACL do not filter traffic within a subnet.
    - So NACL is stateless fiewwalls, so rules must be set for both inbound and outbound traffic
    - NALC rules can either allow ir deny traffic
- **Security Groups**
    - It acts as firewall for individual resources (EC2.LB.RDS)
    - It is the Statefull firewall. Only the direction of the request needs to be permitted.
    - There is no rules to block everything

- **Elastic load balancer**
    - Imagine we have instance that is accessed by end user. If that instance is crashed means we need back up instance.
    - So we create 3 instance. Now if one instance is failed means the other instance will be up but the end used need to change their crashed IP address to new/next avaible IP address.
    - So like wise the user need to switch between them.This is very bad design.
    - So on top of the 3 instance we will have device that also have an ip [dreass.So](http://dreass.So) the user can always point to the device ip address ,that wiill be do the job of instance changing

![image.png](attachment:62a35a1c-bb49-467c-ac5b-a0d68302c7fc:image.png)

3 Types of load balancers

- Classic load balancer
- Application load balancer
- Network Load Balancer

**Classic load balancer**

- First load balancer introduced by AWS
- Not recommended to use
- If two apps are their to load balance in classic their should not have different SSH sign.That is the main drawback.

**Application load balancer**

- It work related on the application layar 4
- It is most specific to Web based applications
- Support HTTP/HTTPS/WEBSOCKET
- Perform application related health checks
- Ex: If user directs to app
- first request goes to load balancer. The load balancer has SSL certificate that will safe request to app(ec2 instance). We can sign SSH the ec2 instance so the request will so as HTTPS

![image.png](attachment:9b8f56f0-4691-46b1-8263-7f0ef44bb283:image.png)

**Network Load Balancer**

- It load bal based on TCP/UDP (layer 4)
- Faster  that Application load balancers
- Healthy checks are only basic ICMP/TCP connections
- NLB forward TCP connections to instances

![image.png](attachment:af56fc7d-4684-4ddf-afd8-41500e481b7a:image.png)

**Workflow of the ELB**

![image.png](attachment:6b026b7c-cd18-49f5-a804-651eb1224334:image.png)

- Full request flow

Step 1: User opens [https://yourapp.com](https://yourapp.com/)

Step 2: DNS resolves [yourapp.com](http://yourapp.com/) → ELB address

Step 3: ELB receives request
Checks which zone is healthy
Decides: "Send to Zone A this time"

Step 4: LB Node in Zone A (Public Subnet)
Forwards to Nginx EC2 in Private Subnet Zone A
port 443 → port 80 (internal)

Step 5: Nginx serves React app OR
proxies /api calls → Node.js EC2 (same zone)

Step 6: Node.js queries RDS MySQL
Response flows back → Nginx → LB Node → User ✅

**Cross-Zone load balancing**

- It is usefull as we can balance the traffic based on the availabilty zone.

![image.png](attachment:f0b5434c-d5e6-4511-9eff-3051a991b956:image.png)

**ELB Architecture**

![image.png](attachment:b9412261-6bb5-4919-9314-e00561375474:image.png)

**Listernrs and Target Group in ELB**

- We can put the listner like by adding our app domain address so if user trigger that point it will send to the scheduled target group

![image.png](attachment:0ee54cc8-83a0-44be-b591-e852e0553686:image.png)

**VPC Peerning**

- Connecting the two VPC
- Not only in same account. we can connect two different aws account VPC.
- One VPC needs to request connection for other VPC

**Transit Gateway**

- It simplify the networking between the VPC
- Allow for transtitive routing
- Must specify the subnet from each avail zone to be used by transit gateway to route traffic
- Can peer with other Transit gateway in different regions or AWS account

**Privatelink**

- Allows resources in our VPC to connect to aws services if they were in the same VPC
- Used to connect to public AWS services (S3, cloudwatch) or to other VPCs in AWS
- VPC endpoints is used here to communicate between the VPC instance and services.
- Regions are located in certain contries
- Not all services will be avaiable in the all regions

## Access of AWS

We have 3 types of aws access

- Console
- Command line interrface
- SDK

## VPC - Virtual Private Cloud

---

It is private isolated network segment hosted withtin aws cloud. the core things which taken care are

- Subnetting (IP Address)
- Routing (Route Tables)
- Firewalls (Security groups)
- Gateways

The VPC is also isolated with the regions. for each regions have diff VPC

![image.png](attachment:2da7715c-3105-4943-aac3-eaa0ef65f25f:image.png)

We have two methods of VPC

- Default
- Custom

Default:

Everyhting basic setting like intertnet connection of the resousece and other are will configured at initial time by the aws.

Custom:

ALl the required steps for the VPC eed to be configured by the ourself. Internet access, ip management everything.

Default setting are

- One default VPC per region
- /16 IPv4 CIDR block 172.31.0.0/16 this means we can access 65536 addreesss
- /20 default subnet in each Availability Zone (4096 address)
- Internet gateway attached to the VPC

AI explantion about VPC

# AWS Networking Basics — Simple Explanation

## Think of it like a City

Imagine AWS is a huge city, and you want to build your own gated community inside it. That gated community is your VPC.

## IP Address — Your Home Address

Just like every house has a unique address, every device/server on a network needs a unique IP address so others can find it.

Example: 192.168.1.10

## CIDR — Defining Your Neighborhood Size

CIDR (Classless Inter-Domain Routing) just defines how many addresses exist in your network.

Think of it like this — when you build a gated community, you need to decide how big it is. Will it have 100 plots or 10,000 plots?

172.31.0.0/16 — the /16 tells you the size. A smaller number = more addresses (bigger neighborhood). A bigger number = fewer addresses (smaller neighborhood).

- /16 = ~65,000 addresses (large community)
- /24 = ~256 addresses (small street)

You don't need to memorize the math — just remember: CIDR defines the size of your network.

## Subnets — Dividing Your Community into Sectors

Your gated community has different zones — residential area, commercial area, restricted staff-only area. Subnets do the same — they divide your VPC into smaller sections.

- Public Subnet = like the main entrance area, visible and accessible from outside (internet-facing)
- Private Subnet = like the inner residential area, not directly accessible from outside (databases, internal apps)

## Internet Gateway — The Main Gate

Your gated community has a main gate that connects it to the outside city (internet). The Internet Gateway (IGW) is exactly that — the door between your VPC and the public internet. Without it, nothing inside can talk to the outside world.

## Route Table — Signboards / Traffic Direction

Inside your community, there are signboards that tell you which road leads where. Route Tables do the same for network traffic. They say things like:

- Traffic going to the internet? Go through the main gate (IGW)
- Traffic staying inside the community? Stay internal

## Security Groups — Security Guard at Your Door

Every house in your community has a security guard at the door who decides who is allowed IN (inbound rules) and who can go OUT (outbound rules). Security Groups work the same way for your AWS resources like EC2 servers. You define rules like:

- Allow web traffic on port 80 from anyone
- Allow SSH access only from your IP
- Block everything else

## Network ACL (NACL) — Community-Level Security Checkpoint

If Security Groups are guards at each house, NACLs are the checkpoint at the entrance of each sector/subnet. They control traffic at a broader level before it even reaches individual resources.

## NAT Gateway — A Proxy for Private Areas

People in the private residential area (private subnet) can't directly go outside through the main gate. But sometimes they need to — like ordering food (software updates, API calls). A NAT Gateway acts like a concierge/proxy — residents make a request, the concierge goes outside on their behalf, gets what's needed, and brings it back. The outside world never directly knows about the private residents.

## Putting It All Together

Internet → Internet Gateway (Main Gate) → VPC - Your Gated Community (172.31.0.0/16)

Inside the VPC:

- Public Subnet (Web servers, Load balancers) → protected by Security Group (Door Guard)
- Private Subnet (Databases, App servers) → protected by Security Group (Door Guard) → uses NAT Gateway to reach internet without being exposed

## Summary

- VPC = Gated Community
- CIDR = Size of the community
- Subnet = Sectors inside the community
- Internet Gateway = Main entrance gate
- Route Table = Signboards/directions
- Security Group = Guard at each house door
- NACL = Checkpoint at sector entrance
- NAT Gateway = Concierge/proxy for private areas
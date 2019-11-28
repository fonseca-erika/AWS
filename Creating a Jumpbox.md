# AWS - Cloud Computing - Configuration of a Jumpbox

## Objective
Create an environment with a public and a private subnet where the internet routing of the private subnet is done through a NAT instance.

![alt tag](https://docs.aws.amazon.com/vpc/latest/userguide/images/nat-instance-diagram.png)

## Setting up the environment

# 1. Creating the VPC and the subnets

Open the Amazon console and search for VPC and access the VPC Dashboard, select Your VPCs in the menu, and click the blue button Create VPC. 

Configure the VPC indicating:
- name tag: choose a name to it, and copy it to a text file to use in later configurations;
- IP CIDR block: choose the IP pattern for your network, indicating how many bits must be blocked for indicating the VPC.  (e.g.: 10.0.0.0/16, where 16 bit are blocked to define de VPC)
- IPv6 CIDR block: keep the default configuration
- Tenancy: keep the default configuration

After filling up the fields, click Create.

The next step is creating the subnets, so in the VPC Dashboard you should select the item Subnets and click the blue button Create subnet.


Configure the Public and the Private subnets indicating:

- name tag: choose a name to it, and copy it to a text file to use in later configurations;
- VPC: select the name of the VPC that you just created;
- Availability zone: choose the option that is more suitable for you application, having in mind the proximity to your end user;
- IPv4 CIDR block: choose the IP pattern for your subnet, respecting the rule already defined in the VPC, indicating how many bits must be blocked for indicating the VPC (e.g.: 10.0.1.0/24, where 24 bit are blocked to define de VPC and the subnet) 

At this stage you have not yet defined the configurations that differ the public and the private subnet, so let's do it in the next steps.

# 2. Configuring the instances

At this stage we need to configure the instances that will be part of the environment.







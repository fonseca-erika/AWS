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

# 2. Setting up the instances

At this stage we need to configure the instances that will be part of the environment.

# 2.1 Configuring the NAT Instance

Navigate to Amazon EC2 Dashboard, and click Launch instance, on the field defined for search type the string amzn-ami-vpc-nat (this will  provide a list for Amazon Linux AMIs available). Select the Community AMIs category, and choose one of the options presented and click on Select.

On the Choose an Instance Type page, select the Free tier eligible, then choose Next: Configure Instance Details.

On the Configure Instance Details page, select the VPC you created from the Network list, and select your public subnet from the Subnet list. Keep the other fields as presented in the default, then choose Next: Add Storage.

Keep the default configuration for the Add Storage, then click Next Add Tags. On the Tags screen also keep the default and choose Next: Configure Security Group.

On the Configure Security Group page, select Create a new security group, and name it as NATSG, later on we will configure this group, so for now just choose Review and Launch.

On the Review Instance Launch choose Launch. You're going to be asked to choose a key pair to launch your instance, choose if you want to create a new one or use an existing and click Launch Instances.

One additional step is to disable the source and destination check, and for this in the EC2 Navigator, select the NAT instance, choose Actions, Networking, Change Source/Dest. Check.

For the NAT instance, verify that this attribute is disabled. Otherwise, choose Yes, Disable.

# 2.2 Configuring an EC2 Instance

Navigate to Amazon EC2 Dashboard, and click Launch instance, choose Amazon Linux 2 AMI (HVM), SSD Volume Type and click on Select.

On the Choose an Instance Type page, select the Free tier eligible, then choose Next: Configure Instance Details.

On the Configure Instance Details page, select the VPC you created from the Network list, and select your private subnet from the Subnet list. Keep the other fields as presented in the default, then choose Next: Add Storage.

Keep the default configuration for the Add Storage, then click Next Add Tags. On the Tags screen also keep the default and choose Next: Configure Security Group.

On the Configure Security Group page, select Create a new security group, and name it as Private SG, later on we will configure this group, so for now just choose Review and Launch.

On the Review Instance Launch choose Launch. You're going to be asked to choose a key pair to launch your instance, choose if you want to create a new one or use an existing and click Launch Instances.

# 3. Networking and Security

# 3.1 Configuring the Internet Gateway

In the navigation pane for the VPC Dashboard, choose Internet Gateways, and then choose Create internet gateway. Name your internet gateway, and then choose Create.

Select the internet gateway that you just created in the list, and then choose Actions, Attach to VPC. Select the VPC that you created from the list, and then choose Attach.

# 3.2 Setting an Elastic IP

In the navigation pane for the VPC Dashboard, choose Elastic IPs, and then choose Allocate new address. Choose Allocate.

Select the Elastic IP address from the list, and then choose Actions, Associate address.

Select the network interface resource, then select the network interface for the NAT instance that you created. Select the address to associate the Elastic IP with from the Private IP list, and then choose Associate.

# 3.2 Configuring the route table for the public network

In the navigation pane for the VPC Dashboard, choose Route Tables, and then choose Create Route Table.

In the Create Route Table dialog box, name your route table and copy this name to use later on, then select your VPC, and then choose Yes, Create.

Select the custom route table that you just created. The details pane displays tabs for working with its routes, associations, and route propagation.

On the Routes tab, choose Edit, Add another route, and add the following routes as necessary. Choose Save when you're done.

For IPv4 traffic, specify 0.0.0.0/0 in the Destination box, and select the internet gateway ID in the Target list.

On the Subnet Associations tab, choose Edit, select the Associate check box for the public subnet that you created previously, and then choose Save.

# 3.3 Configuring the route table for the private network

In the navigation pane for the VPC Dashboard, choose Route Tables, and then choose Create Route Table.

In the Create Route Table dialog box, name your route table and copy this name to use later on, then select your VPC, and then choose Yes, Create.

Select the custom route table that you just created. The details pane displays tabs for working with its routes, associations, and route propagation.

On the Routes tab, choose Edit, Add another route, and add the following routes as necessary. Choose Save when you're done.

For IPv4 traffic, specify 0.0.0.0/0 in the Destination box, and select the NAT instance in the Target list.

On the Subnet Associations tab, choose Edit, select the Associate check box for the private subnet that you created previously, and then choose Save. (By doing this you are sending the traffic of Internet requests to the NAT gateway.)

# 3.4 Configuring the security group for the NAT Instance

In the VPC Dashboard, choose Security Groups, and then select the NATSG Security Group that you created previously. The details pane displays the details for the security group, plus tabs for working with its inbound and outbound rules.

Add rules for inbound traffic using the Inbound Rules tab as follows:

Choose Add another rule, and select HTTP from the Type list. In the Source field, specify the IP address range of your private subnet.

Choose Add another rule, and select HTTPS from the Type list. In the Source field, specify the IP address range of your private subnet.

Choose Add another rule, and select SSH from the Type list. In the Source field, specify 0.0.0.0/0

Choose Save.

Add rules for outbound traffic using the Outbound Rules tab as follows:

Choose Add another rule, and select HTTP from the Type list. In the Destination field, specify 0.0.0.0/0

Choose Add another rule, and select HTTPS from the Type list. In the Destination field, specify 0.0.0.0/0

Choose Save.

# 3.4 Configuring the security group for the Instance in the Private Subnet

In the VPC Dashboard, choose Security Groups, and then select the Private SG that you created previously. The details pane displays the details for the security group, plus tabs for working with its inbound and outbound rules.

Add rules for inbound traffic using the Inbound Rules tab as follows, choose Edit and configure:

Choose Add another rule, and select SSH from the Type list. In the Source field, specify the IP of your public network (choose the elastic IP just created)

Choose Save.

Add rules for outbound traffic using the Outbound Rules tab as follows:

Choose Add another rule, and select All ICMP - IPv4 from the Type list. In the Destination field, specify 0.0.0.0/0

Choose Save.

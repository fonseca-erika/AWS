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

On the Configure Security Group page, select the Select an existing security group option, and select the NATSG security group that you created. Choose Review and Launch.

Review the settings that you've chosen. Make any changes that you need, and then choose Launch to choose a key pair and launch your instance.

(Optional) Connect to the NAT instance, make any modifications that you need, and then create your own AMI that's configured to run as a NAT instance. You can use this AMI the next time that you need to launch a NAT instance. For more information see Creating Amazon EBS-Backed AMIs in the Amazon EC2 User Guide for Linux Instances.

You can use the VPC wizard to set up a VPC with a NAT instance; for more information, see VPC with Public and Private Subnets (NAT). The wizard performs many of the configuration steps for you, including launching a NAT instance, and setting up the routing. However, if you prefer, you can create and configure a VPC and a NAT instance manually using the steps below.

Create a VPC with two subnets.

Note

The steps below are for manually creating and configuring a VPC; not for creating a VPC using the VPC wizard.

Create a VPC (see Creating a VPC)

Create two subnets (see Creating a Subnet)

Attach an Internet gateway to the VPC (see Creating and Attaching an Internet Gateway)

Create a custom route table that sends traffic destined outside the VPC to the Internet gateway, and then associate it with one subnet, making it a public subnet (see Creating a Custom Route Table)

Create the NATSG security group (see Creating the NATSG Security Group). You'll specify this security group when you launch the NAT instance.

Launch an instance into your public subnet from an AMI that's been configured to run as a NAT instance. Amazon provides Amazon Linux AMIs that are configured to run as NAT instances. These AMIs include the string amzn-ami-vpc-nat in their names, so you can search for them in the Amazon EC2 console.

Open the Amazon EC2 console.

On the dashboard, choose the Launch Instance button, and complete the wizard as follows:

On the Choose an Amazon Machine Image (AMI) page, select the Community AMIs category, and search for amzn-ami-vpc-nat. In the results list, each AMI's name includes the version to enable you to select the most recent AMI, for example, 2013.09. Choose Select.

On the Choose an Instance Type page, select the instance type, then choose Next: Configure Instance Details.

On the Configure Instance Details page, select the VPC you created from the Network list, and select your public subnet from the Subnet list.

(Optional) Select the Public IP check box to request that your NAT instance receives a public IP address. If you choose not to assign a public IP address now, you can allocate an Elastic IP address and assign it to your instance after it's launched. For more information about assigning a public IP at launch, see Assigning a Public IPv4 Address During Instance Launch. Choose Next: Add Storage.

You can choose to add storage to your instance, and on the next page, you can add tags. Choose Next: Configure Security Group when you are done.

On the Configure Security Group page, select the Select an existing security group option, and select the NATSG security group that you created. Choose Review and Launch.

Review the settings that you've chosen. Make any changes that you need, and then choose Launch to choose a key pair and launch your instance.

(Optional) Connect to the NAT instance, make any modifications that you need, and then create your own AMI that's configured to run as a NAT instance. You can use this AMI the next time that you need to launch a NAT instance. For more information see Creating Amazon EBS-Backed AMIs in the Amazon EC2 User Guide for Linux Instances.

Disable the SrcDestCheck attribute for the NAT instance (see Disabling Source/Destination Checks)

If you did not assign a public IP address to your NAT instance during launch (step 3), you need to associate an Elastic IP address with it.

Open the Amazon VPC console at https://console.aws.amazon.com/vpc/.

In the navigation pane, choose Elastic IPs, and then choose Allocate new address.

Choose Allocate.

Select the Elastic IP address from the list, and then choose Actions, Associate address.

Select the network interface resource, then select the network interface for the NAT instance. Select the address to associate the Elastic IP with from the Private IP list, and then choose Associate.

Update the main route table to send traffic to the NAT instance. For more information, see Updating the Main Route Table.





# AWS - Configuration of a Jupyter Notebook Server in Ubuntu

## Objective
Create an environment with a Jupyter Notebook Server in the Ubuntu OS, accessible to users through Internet.

![alt tag](https://geohackweek.github.io/datasharing/fig/geohackweek_aws_setup.png)

## Setting up the environment

## 1. Configuring the network

Set up the network environment providing a VPC connected to an Internet gateway, and define a route table that allows incoming access from the Internet.

Configure a security that allows access to the port 8888, that is the one that will be used for Jupyter Notebook Server once your instance is available.

Allocate an elastic IP that will be used by your EC2 instance.

## 2. Configuring the EC2 Instance

### 2.1 Launching the instance

Navigate to Amazon EC2 Dashboard, and click Launch instance, choose Ubuntu Server 18.04 LTS (HVM), SSD Volume Type and click on Select.

On the Choose an Instance Type page, select the Free tier eligible, then choose Next: Configure Instance Details.

On the Configure Instance Details page, select the VPC you created from the Network list, and select your private subnet from the Subnet list. Keep the other fields as presented in the default, then choose Next: Add Storage.

Keep the default configuration for the Add Storage, then click Next Add Tags. On the Tags screen also keep the default and choose Next: Configure Security Group.

On the Configure Security Group page, choose Select an existing security group, and select in the list the one that you just configured, then choose Review and Launch.

On the Review Instance Launch choose Launch. You're going to be asked to choose a key pair to launch your instance, choose if you want to create a new one or use an existing and click Launch Instances.

### 2.2 Configuring the Jupyter Notebook server

<i><b>Remark: </b> JupyterLab can be installed using conda or pip, and in this tutorial our alternative chosen is to use pip.</i>

Connect to the Ubuntu EC2 instance an run the following command line:

sudo apt update

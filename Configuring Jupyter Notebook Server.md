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

In the navigation pane for the VPC Dashboard, choose Elastic IPs, and then select the Elastic IP address from the list, and then choose Actions, Associate address.

Select the Instance option, then select the Ubuntu EC2 instance that you created, and then choose Associate.

### 2.2 Configuring the Jupyter Notebook server

<i><b>Remark: </b> JupyterLab can be installed using conda or pip, and in this tutorial our alternative chosen is to use pip. Remember that pip is package that manages the installation of other packages written in Python.</i>

Connect to the Ubuntu EC2 instance an run the following command line:

sudo apt update

Use the following command to check if python is installed on your environment:

python3 --version

If it's not installed, run the following command:

sudo apt-get install python3

It's necessary to import pip and after install Jupyter, as it's done typing the following command lines:

curl -O https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py --user

sudo apt install python3-pip
python3 -m pip install --upgrade pip
python3 -m pip install jupyter --user

To run the Jupyter notebook server you can use the following command line:

nohup jupyter notebook --ip=0.0.0.0

<i><b>Remark:</b> 
- if you neglect the ip parameter the server will run only in the local host;
- to guarantee that your Jupyter Notebook server will not be terminated when you close you ssh session, and to run it in background you need to:</i>

Now, you can try to access your Jupyter Notebook server through your web browser, using the the port and token provided by the configuration. If it fails, try to review the security group configuration.

### 2.3 Configuring virtual environment

Below there are the steps for configuring a virtual environment, that is useful to isolate different projects requirements, ans avoiding conflict among packages.

First you need to install the virtualenv package that allows you to deal with virtual environment, to do so, type the following command:

sudo pip3 install virtualenv

The next step is to define the path to the system store the files of this new environment, in this example we are creating a directory (using mkdir) and attaching it to be the location of the new virtual environment that we want to create:

mkdir my_env
virtualenv my_env

The next step is to activate this environment, and by doing so it can be accessed through the command line:

source my_env/bin/activate

To add the virtualenv as a jupyter kernel the following command must be run:

ipython kernel install --name "my_env" --user

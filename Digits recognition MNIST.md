# AWS - Cloud Computing - Architecture for Machine Learning on Cloud

## Objective
Create a complete architecture to predict digits drawn by the user, composed of:

- Instance to make predictions
- Webserver to make call of the predictions
- NAT Instance to allow a route between to Internet 

The requirement is that the instances used to train and to make predictions must be in a private network.

## Setting up the environment

# 1. Creating the machine for the prediction

sudo apt-get update
sudo apt install python3
sudo apt install pip3
pip3 install tensorflow
pip3 install keras
pip3 install flask
pip3 install imageio
pip3 install flask_cors
pip3 install pillow
pip3 install numpy

git clone https://github.com/leodsti/AWS_Tutorials.git
cd AWS_Tutorials/MNIST/

The following line will open the file, that needs to be edited:
sudo nano keras_flask.py

Add the following lines to the top of the file:

from flask_cors import CORS
from keras import backend as K

app = Flask(__name__)
cors = CORS(app, resources=(r"*": {"origins": "*"}})

You need to do some changes on the following code:
global model, graph
model = load_model('./cnn-mnist')
graph = graph = tf.get_default_graph()

So it will become:
global model
model = load_model('./cnn-mnist')
graph = graph = tf.get_default_graph()

Now go to the predict function:
def predict():

And add at the top the following line:
  global graph

CTRL+O+ENTER
CTRL+X

python3 keras_flask.py

The subnet should be configured on the Security Group to access calls on PORT: 5000


# 2. Creating a NAT instance

Create a NAT instance and associate it to an Elastic IP. Take not of the elastic IP number.

# 2. Creating Web Interface

Install an EC2 instance with Ubuntu, and configure it to be an WebServer.

## Installing the webserver 
sudo apt install apache2

#check if apache2 not running:
sudo systemctl status apache2

#if it is not running type this:
sudo systemctl start apache2

## Code

The code you need to run in your we interface is availa files you need on LeoDSTI

git clone https://github.com/leodsti/AWS_Tutorials.git
cd AWS_Tutorials/MNIST/
sudo mv AWS_Tutorial/MNIST/index.html /var/www/html
sudo mv AWS_Tutorial/MNIST/static/index.js /var/www/html 
sudo mv AWS_Tutorial/MNIST/static/style.css /var/www/html

cd /var/www/html

The following line will open the file, that needs to be edited:
sudo nano index.html

Go to the following line:
url: "http://34.249.220.182:5000/predict/"

Change the IP and the port:
url: "http://<ELASTIC IP>:5000/predict/"

CTRL+O+ENTER
CTRL+X

sudo systemctl restart apache2






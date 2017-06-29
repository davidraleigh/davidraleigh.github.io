
# Debugging a Remote Docker Container with PyCharm  

Say I have a Docker container running in Google Cloud Platform. Inside that container a custom library interacts with huge amounts of data from Google Cloud Storage. In order to debug that library without suffering from latency issues or egress costs I would need to ssh into the VM and from there use `docker exec` to get into the container. Then I could debug my library using vim or emacs. 

But if I want to use the Remote Debugger feature of PyCharm it becomes a bit more complicated. Below are the hacked together steps for having a GCP development machine host a Docker container where my library can be debugged with PyCharm.

### Firewall Rules
First off you'll need to create a new firewall rule for your project so you can ssh into the Docker Container's port. We'll use the port number 52022. Go to the Table of Contents in Google cloud and select Networking:
![Networking/Firewall](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/pycharm-remote-debug/firewall-settings-1.png)

The fields you'll have to edit are `Name`, `Target Tags`, `Source IP ranges`, and `Protocols and ports`. You should add a description, but that's optional. If you want to specify that only your IP address can access the machine you should define the `Source IP ranges` as something besides `0.0.0.0/0`. Below you can see all the settings I've used, if you copy all these settings this tutorial should work:
![Specific SSH Firewall Settings](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/pycharm-remote-debug/firewall-settings-2.png)

### Create VM with Proper Permissions


### Git Clone your Code onto the VM
An alternative is for you to build your image, push it to Google Container Registry and pull it to your dev machine

### Copy Debug Dockerfile, supervisord Configurations and Pycharm Helpers


### Setup Pycharm Development Environment


### Rebuilding Image


# Debugging a Remote Docker Container with PyCharm  

Say I have a Docker container running in Google Cloud Platform. Inside that container a custom library interacts with huge amounts of data from Google Cloud Storage. In order to debug that library without suffering from latency issues or egress costs I would need to ssh into the VM and from there use `docker exec` to get into the container. Then I could debug my library using vim or emacs. 

But if I want to use the Remote Debugger feature of PyCharm it becomes a bit more complicated. Below are the hacked together steps for having a GCP development machine host a Docker container where my library can be debugged with PyCharm.

### Firewall Rules
First off you'll need to create a new firewall rule for your project.

### Create VM with Proper Permissions


### Git Clone your Code onto the VM
An alternative is for you to build your image, push it to Google Container Registry and pull it to your dev machine

### Copy Debug Dockerfile, supervisord Configurations and Pycharm Helpers


### Setup Pycharm Development Environment


### Rebuilding Image

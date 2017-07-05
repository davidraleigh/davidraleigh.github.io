
# Debugging a Remote Docker Container with PyCharm  

Say I have a Docker container running in Google Cloud Platform. Inside that container a custom library interacts with huge amounts of data from Google Cloud Storage. In order to debug that library without suffering from latency issues or egress costs I would need to ssh into the VM and from there use `docker exec` to get into the container. Then I could debug my library using vim or emacs. 

But if I want to use the Remote Debugger feature of PyCharm it becomes a bit more complicated. Below are the hacked together steps for having a GCP development machine host a Docker container where my library can be debugged with PyCharm.

To complete this tutorial you'll need a GCP account with Admin access, you'll need to have gcloud tools installed on your development machine, and you'll need to have PyCharm Professional (the standard free edition doesn't have remote debugging installed).

### Firewall Rules
First off you'll need to create a new firewall rule for your project so you can ssh into the Docker Container's port. We'll use the port number 52022. Go to the table of contents in Google cloud and select Networking and then the Firewall Rules:
![Networking/Firewall](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/pycharm-remote-debug/firewall-settings-1.png)

The fields you'll have to edit are `Name`, `Target Tags`, `Source IP ranges`, and `Protocols and ports`. You should add a description, but that's optional. If you want to specify that only your IP address can access the machine you should define the `Source IP ranges` as something besides `0.0.0.0/0`. Below you can see all the settings I've used, if you copy all these settings this tutorial should work:
![Specific SSH Firewall Settings](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/pycharm-remote-debug/firewall-settings-2.png)

### Create VM with Proper Permissions
Go to Compute Engine in GCP console table of contents and select `Create an Instance`. You'll want to change the `Boot disk` to be a docker enabled image (in this case the ChromiumOS) and maybe increase the size if you plan on using this for development of lots of different docker images:

![Boot Disk Selection](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/pycharm-remote-debug/container-optimized-disk.png)

Under the `Firewall` section of your instance creation dialog select the `Allow HTTP traffic` field. You may need to check with your Networking Firewall rules to make sure that port 80 is open for your IP address (by default GCP projects make it open to all addresses). Below the `Firewall` section you'll need to select the `Networking` tab and place the `Target Tags` you defined earlier in the Firewall section of this tutorial (mine was `container-ssh`) in the `Network tags` field:

![Network Tags](https://github.com/davidraleigh/davidraleigh.github.io/blob/master/assets/pycharm-remote-debug/network-tags.png)

Once you've finished these settings `Create` your VM and you should be ready to get your VM ready for debugging.

### Get a Docker Image with Updated Source Code on your Development VM

#### Git Clone your Code onto the VM
SSH into your VM and use git to clone the following repo:
```bash
sudo mkdir /opt/src
cd /opt/src
sudo git clone https://github.com/davidraleigh/blog-remote-debug-python
```

Once your source code is there you'll want to build your Docker image:
```bash
cd /opt/src/blog-remote-debug-python
sudo docker build -t repo-image .
```

#### Pull an Updated Image
From Docker Hub Registry or from Google Container Registry you can pull an image with recent source code for your project to your Development VM (this example uses GCR):
```bash
sudo gcloud docker -a
sudo gcloud docker pull gcr.io/your-project-name/repo-image
sudo docker tag gcr.io/your-project-name/repo-image repo-image
```

### Copy Debug Dockerfile, supervisord Configurations and Pycharm Helpers

If you want to use a ssh key other than the one issued to you when you installed and logged on to your project using `gcloud auth login`, `gcloud auth activate-service-account` and `gcloud config set project`


### Setup Pycharm Development Environment


### Rebuilding Image

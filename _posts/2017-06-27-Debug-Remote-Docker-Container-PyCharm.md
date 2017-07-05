
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

### Get a Docker Image with Updated Source Code on your Remote Development VM
You can either SSH onto your machine and clone the repo to debug onto your VM and build it (great if you have a slow connection at your local machine and you don't want to push an image into your container registry). Or you can build your test image locally, push it to your container registry of choice and then pull from that registry onto your remote VM.

#### Git Clone your Code onto the Remote Development VM
SSH into your remote VM (in my cast `remote-debug-demo`) and use git to clone the following repo:
```bash
# from your local machine use gcloud to ssh in
gcloud compute --project "blog-and-demos" ssh --zone "us-central1-f" "remote-debug-demo"
sudo mkdir /opt/src
cd /opt/src
sudo git clone https://github.com/davidraleigh/blog-remote-debug-python
```

Once your source code is there in your remote VM you'll want to build your Docker image:
```bash
cd /opt/src/blog-remote-debug-python
sudo docker build -t your-special-image-name .
sudo docker tag your-special-image-name test-image
```

#### Pull an Updated Image to Your Remote Development VM
If you've already pushed your test image to an image repository  (Docker Hub Registry, Google Container Registry, etc.)then you can SSH onto your remote VM and pull it down (this example uses GCR):
```bash
# from your local machine use gcloud to ssh in
gcloud compute --project "blog-and-demos" ssh --zone "us-central1-f" "remote-debug-demo"
sudo gcloud docker -a
sudo gcloud docker pull gcr.io/your-project-name/your-special-image-name
sudo docker tag gcr.io/your-project-name/your-special-image-name test-image
```

### Create a Debug Image
In order to debug your code with PyCharm you must be able to SSH into the running docker container. Rather than screw up your project's Dockerfile, we'll just use a Dockerfile that inherits from the image you want to use as your remote debugging image.

The easiest way to do this is to use the Dockerfile and associated supervisord configuration files from the https://github.com/davidraleigh/remote-debug-docker repo. In your VM, clone this repo and follow the repo's instructions:

```bash
# from your local machine use gcloud to ssh in
gcloud compute --project "blog-and-demos" ssh --zone "us-central1-f" "remote-debug-demo"
cd ~/
sudo git clone https://github.com/davidraleigh/remote-debug-docker
```

In order to build a Docker image that has the ssh public key to approve your request you'll need to print it to the `a uthorized_keys` fiel. Copy your google cloud `google_compute_engine.pub` public ssh key to your remote VM. It is the key that google created when you installed `gcloud` and setup your account and configuration for GCP (at some point you should have executed the following commands: `gcloud auth login`, `gcloud auth activate-service-account` and `gcloud config set project`). So from your local dev maching you'll execute the following commands:
```bash
# remote-debug-docker should have been created in your previous ssh and git clone in your remote VM
gcloud compute copy-files ~/.ssh/google_compute_engine.pub davidraleigh@remote-debug-demo:/home/davidraleigh/remote-debug-docker/ --zone=us-central1-f
```

The last thing you'll need for this all to work is to get a hold of the pycharm remote debug helpers that Pycharm installs on any remote debug machine. This is a little tricky. how I've done this in the past is that I've setup a remote debug VM with PyCharm and then gone into that remote VM and copied the ~/.pycharm_helpers to a google storage location for later use. It'd be nice if pycharm just provided a distribution location for that. I had one in a location calle

Copy Debug Dockerfile, supervisord Configurations and Pycharm Helpers


If you want to use a ssh key other than the one issued to you when you installed and logged on to your project using `gcloud auth login`, `gcloud auth activate-service-account` and `gcloud config set project`


### Setup Pycharm Development Environment


### Rebuilding Image

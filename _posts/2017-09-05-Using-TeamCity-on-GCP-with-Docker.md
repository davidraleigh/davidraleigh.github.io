
Create Instance
![Create an Instance](https://davidraleigh.github.io/assets/teamcity-server-create/create-instance-with-disk.png)


Install Docker
```bash
sudo apt-get remove docker docker-engine docker.io
sudo apt-get update
sudo apt-get install     linux-image-extra-$(uname -r)     linux-image-extra-virtual
sudo apt-get update
sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
curl -fsSL https://download.docker.com/linux/ubuntu/gpg
sudo apt-key fingerprint 0EBFCD88
echo $(lsb_release -cs)
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
apt-cache madison docker-ce
sudo apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial
sudo docker run hello-world
```

# SD-TwizyModel
This repository contains all instructions to deploy your docker to run the SD-TwizyModel.

## 1. Installing docker
```
sudo apt update
sudo apt-get install  curl apt-transport-https ca-certificates software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
```

## 2. Clone this repository and access the folder
```
git clone https://github.com/UFG-DINOLab/dockerfiles.git
cd dockerfiles/SD-TwizymModel
```
## 3. Build your dockerfile image
```
docker build -t ubuntu16.04-ros-kinetic .
```
## 4. Run!
```
sudo docker run -it --name ubuntu1604 -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix ubuntu16.04-ros-kinetic
```

<h2> Install docker-ce </h2> <img src="https://user-images.githubusercontent.com/20130001/86041084-c2717c00-ba62-11ea-9437-120d650c88d4.png " alt="drawing" width="200"/>

#### Note do not install docker.io it's old

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
```
#### Check docker is up and running 
```
docker -v
sudo systemctl status docker 

//if it's not up and running 
sudo systemctl enable docker
sudo systemctl start docker
```
#### Add docker to sudo [optional]
```
sudo usermod -aG docker sudo
```
# Docker as agent in Jenkins to reduce cost and efficient

## Install Docker in jenkins machine

~~~ bash
sudo apt update
sudo apt install docker.io -y
~~~

### Docker-slave configuration
~~~bash
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
~~~




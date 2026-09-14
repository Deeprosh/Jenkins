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

### Install docker plugins to run docker as agent in jenkins
- Go to manage clouds and select the docker pipeline and install it.
- Restart it after installing the pipeline for the jenkins to connect with docker daemon process.
- Write your first jenkins pipeline by going to **item** from the dashboard and give pipeline name and select **pipeline** from the option to write your jenkins pipeline.


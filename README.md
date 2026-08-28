# Jenkins Installation & CI/CD Setup

A hands-on guide to installing and configuring **Jenkins on an AWS EC2 instance**, integrating Docker as an agent, creating CI/CD pipelines, and deploying applications to Kubernetes.

## 🚀 What You'll Learn

* Launch and configure an AWS EC2 instance
* Install Java and Jenkins
* Configure Jenkins securely
* Access Jenkins through the AWS Security Group
* Configure Jenkins plugins
* Integrate Docker with Jenkins
* Create CI/CD pipelines
* Build and push Docker images
* Deploy applications to Kubernetes
* Automate application deployments using Jenkins

---

# ☁️ AWS EC2 Instance

Jenkins will be installed on an Ubuntu EC2 instance.

## Step 1: Launch an EC2 Instance

1. Log in to the **AWS Management Console**.
2. Navigate to **EC2**.
3. Click **Instances**.
4. Click **Launch instances**.
5. Select **Ubuntu Server** as the operating system.
6. Choose an appropriate instance type.
7. Create or select an existing key pair.
8. Configure the Security Group.
9. Configure your internet gateway as well which should be attached to your VPC else it wont let you connect with anything.(This is the biggest challenge i faced when connecting to EC2 instance)
<img width="1561" height="330" alt="image" src="https://github.com/user-attachments/assets/df6dd3ae-c97d-4f3c-b86c-8cee56532596" />


### Recommended Inbound Rules

| Type       | Port | Source          |
| ---------- | ---: | --------------- |
| SSH        |   22 | Your IP address |
| Custom TCP | 8080 | Your IP address |

> ⚠️ For learning purposes, you may temporarily use `0.0.0.0/0` for port `8080`, but restricting access to your IP address is more secure.
## Step 2: Connecting to EC2 Instance through SSH
```
sudo cp "/mnt/c/Users/Roshini somireddy/Downloads/jen.pem" ~/.ssh

chmod 400 pemfile name

sudo ssh -i "pemfile path" ubuntu@ec2 public ip address
```

Challenges faced: 
Need to unblock pem file using its properties.
I'm using wsl terminal in my windows 11 laptop, i copied the path to folder first then tried ssh, it helped me to understand how things actually work in WSL.  

<img width="1085" height="742" alt="image" src="https://github.com/user-attachments/assets/141e9e57-5271-4a66-a246-b3b9d627e61c" />


---

# ☕ Install Java

Jenkins requires Java to run.

Update the package repository:

```bash
sudo apt update
```

Install OpenJDK 17:

```bash
sudo apt install -y openjdk-17-jre
```

Verify the installation:

```bash
java -version
```

You should see output similar to:

```text
openjdk version "17.x.x"
```

---

# 🔧 Install Jenkins

Add the Jenkins repository key:

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

Add the Jenkins repository:

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Update the package repository:

```bash
sudo apt update
```

Install Jenkins:

```bash
sudo apt install -y jenkins
```

---

# ▶️ Start Jenkins

Start the Jenkins service:

```bash
sudo systemctl start jenkins
```

Enable Jenkins to start automatically after a reboot:

```bash
sudo systemctl enable jenkins
```

Check the Jenkins service status:

```bash
sudo systemctl status jenkins
```

If Jenkins is running successfully, you should see:

```text
Active: active (running)
```

---

# 🔐 Configure AWS Security Group

By default, Jenkins listens on port `8080`.

Go to:

**AWS Console → EC2 → Instances → Your Instance → Security → Security Groups**

Add an inbound rule:

```text
Type: Custom TCP
Port: 8080
Source: Your IP address
```

For temporary testing, you can use:

```text
0.0.0.0/0
```

> ⚠️ Avoid leaving Jenkins exposed to the entire internet in a production environment. Prefer restricting access by IP, VPN, load balancer, or reverse proxy.

---

# 🌐 Access Jenkins

Open your browser and navigate to:

```text
http://<EC2-PUBLIC-IP>:8080
```

For example:

```text
http://44.200.0.49:8080
```

Replace the IP address with your EC2 instance's public IP address.

---

# 🔑 Get the Initial Jenkins Administrator Password

Run the following command on the EC2 instance:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it into the Jenkins setup page.

---

# 🧩 Jenkins Initial Setup

After entering the administrator password:

1. Click **Install suggested plugins**.
2. Wait for the plugins to finish installing.
3. Create your first administrator account.
4. Configure the Jenkins URL if prompted.
5. Finish the setup.

You should now see the Jenkins dashboard.

---


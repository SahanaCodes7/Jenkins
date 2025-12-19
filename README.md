````markdown
# Jenkins CI/CD on AWS EC2

This repository demonstrates a complete Jenkins setup on AWS EC2, designed to create a CI/CD-ready environment using Docker-based build agents.  
The project highlights practical DevOps implementation commonly used in real-world production pipelines.

Key objectives include installing Jenkins, configuring Docker as a Jenkins agent, enabling CI/CD workflows, and preparing the setup for Kubernetes-based deployments.

---

## AWS EC2 Instance Setup

- Log in to the AWS Management Console  
- Navigate to **EC2 → Instances**  
- Launch a new **Ubuntu-based EC2 instance**

<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

---

## Jenkins Installation

### Prerequisites
- Java (OpenJDK)

### Install Java

```bash
sudo apt update
sudo apt install openjdk-17-jre
````

Verify installation:

```bash
java -version
```

---

### Install Jenkins

```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins
```

---

## Network Configuration

Jenkins runs on **port 8080**, which must be allowed in the EC2 security group.

### Steps:

* EC2 → Instances → Select your instance
* Open the **Security** tab
* Edit inbound rules
* Allow **TCP port 8080**

<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">

---

## Access Jenkins

Open Jenkins in the browser using:

```text
http://<EC2_PUBLIC_IP>:8080
```

> You can find the EC2 public IP address in the AWS EC2 console.

### Initial Setup

After accessing Jenkins:

* Retrieve the initial admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

* Paste the password into the Jenkins setup screen
* Install suggested plugins
* Create an admin user (recommended)

<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

---

### Install Suggested Plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for Jenkins to complete the plugin installation.

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

---

### Create Admin User

Creating an admin user is recommended for long-term and production-like usage.

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins installation is now complete and ready for use.

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

---

## Install Docker Pipeline Plugin

To enable Docker-based builds in Jenkins:

* Log in to Jenkins
* Go to **Manage Jenkins → Manage Plugins**
* Open the **Available** tab
* Search for **Docker Pipeline**
* Install the plugin and restart Jenkins

<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for Jenkins to restart after plugin installation.

---

## Docker Agent Configuration

### Install Docker

```bash
sudo apt update
sudo apt install docker.io
```

---

### Grant Docker Permissions

Add Jenkins and Ubuntu users to the Docker group:

```bash
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
```

Restart Jenkins to apply permission changes:

```text
http://<EC2_PUBLIC_IP>:8080/restart
```

Docker is now successfully configured as a Jenkins build agent.

---

## Project Outcome

* Jenkins installed and configured on AWS EC2
* Docker integrated as a Jenkins build agent
* CI/CD-ready infrastructure established
* Foundation prepared for Kubernetes and advanced DevOps workflows

---

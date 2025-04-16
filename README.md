# Secure CI/CD Pipeline with Jenkins for Java Web App 🚀  

In my last academic project, I built a **secure CI/CD pipeline using Jenkins** for a Java-based web application. This repository documents every step of the process, from infrastructure setup to deployment. Hope you find it helpful!  

## Project Architecture 📐  
*(Thumbnail of the architecture diagram will be added here)*  

![Architecture Diagram](images/Screenshot%202025-04-16%20011223.png)  

---

## Infrastructure Setup  

### Step 1: Provisioning VMs  
To host the pipeline components, I launched **3 EC2 instances** on AWS:  
1. **Jenkins Server** (CI/CD orchestration)  
2. **SonarQube Server** (Static code analysis)  
3. **Monitoring Machine** (Logs/metrics)  
![Architecture Diagram](images/image.png)  
For Kubernetes, I used **Azure Kubernetes Service (AKS)**.  

![Architecture Diagram](images/Screenshot%202025-04-16%20130900.png)   

---

## Jenkins Configuration  

### Step 2: Install & Set Up Jenkins  
Run these shell commands on your Jenkins server :  

```bash
# Update packages
sudo apt-get update

# Install Java 17+ (Jenkins dependency)
sudo apt install openjdk-17-jre-headless -y

# Add Jenkins repository key
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# Add Jenkins repo
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt-get update
sudo apt-get install jenkins -y

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins (this will start Jenkins anytime we run the virtual machine)
```
### Install Docker  

```bash
#!/bin/bash

# Update package manager repositories
sudo apt-get update

# Install necessary dependencies
sudo apt-get install -y ca-certificates curl

# Create directory for Docker GPG key
sudo install -m 0755 -d /etc/apt/keyrings

# Download Docker's GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

# Ensure proper permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add Docker repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package manager repositories
sudo apt-get update

sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
```
## SonarQube configuration
### step 3:install and set up SonarQube

we need to install docker, so copy the same shell code for jenkins section inside a file.sh and then run it (don't forget chmod +x file.sh)

#### create a sonarqube container
```bash
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```

this will run a sonarqube container in port 9000

### step 4:Create a kubernetes server
you have your own choice if you choose a complete kuberntes service from aws or azure that already have every thing installed and ready or you can create your own kuberenetes server in EC2 VM, for me i've choosed the azure kuberenets service since i have a student account.

### step 5: building the pipeline :
we need to install some plugins befor we start building the pipeline
at least we need to install those plugins:
-Eclipse Temurin Installer
-Pipeline Maven Integration
-SonarQube Scanner
-Kuberenetes CLI
-Kuberentes
-Docker
-Docker Pipeline Step

we need to configure the credentials settings also :
![Architecture Diagram](images/Screenshot%202025-04-16%20144952.png)

then,configure the system and tools parametres.

for the **pipeline** you can find it attached above.

finaly, building the pipeline as you can see it's successfully builded !!
![Architecture Diagram](images/Screenshot%202025-04-15%20204955.png)

and this is the Public IP address provided by Kubernetes to access the webapp.
![Architecture Diagram](images/Screenshot%202025-04-16%20144508.png)






# Secure CI/CD Pipeline with Jenkins for Java Web App 🚀  

In my last academic project, I built a **secure CI/CD pipeline using Jenkins** for a Java-based web application. This repository documents every step of the process, from infrastructure setup to deployment. Hope you find it helpful!  

## Project Architecture 📐  
*(Thumbnail of the architecture diagram will be added here)*  

![Architecture Diagram](images/image.png)  

---

## Infrastructure Setup  

### Step 1: Provisioning VMs  
To host the pipeline components, I launched **3 EC2 instances** on AWS:  
1. **Jenkins Server** (CI/CD orchestration)  
2. **SonarQube Server** (Static code analysis)  
3. **Monitoring Machine** (Logs/metrics)  

For Kubernetes, I used **Azure Kubernetes Service (AKS)**.  

*(Screenshot of the AWS/Azure setup will be added here)*  

---

## Jenkins Configuration  

### Step 2: Install & Set Up Jenkins  
Run these shell commands on your Jenkins server (copy-friendly):  

```bash
# Update packages
sudo apt-get update

# Install Java (Jenkins dependency)
sudo apt-get install openjdk-11-jdk -y

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
sudo systemctl enable jenkins

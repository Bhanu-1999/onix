CI/CD Pipeline Setup for Onix Website
This project demonstrates a complete CI/CD pipeline using Jenkins, SonarQube, and Docker deployed on separate EC2 instances.

Overview
The pipeline automates code integration, quality analysis, and deployment to a Docker containerized environment. The following components are used:

Jenkins: Automates the build, test, and deployment processes.
SonarQube: Performs static code analysis.
Docker: Hosts the application in a containerized environment for deployment.
Infrastructure Setup
EC2 Instances
Three EC2 instances are created using the same key pair:

Jenkins Server (t2.medium)
SonarQube Server (t2.medium)
Docker Server (t2.medium)
Instance Configuration
Jenkins Server Setup
Update the server:
bash
Copy code
sudo apt update  
Install Java 17:
bash
Copy code
sudo apt install openjdk-17-jre  
Set hostname:
bash
Copy code
sudo hostnamectl set-hostname jenkins  
Install Jenkins following Jenkins documentation.
SonarQube Server Setup
Install Java 17 and unzip:
bash
Copy code
sudo apt update  
sudo apt install openjdk-17-jre unzip  
Download and extract SonarQube:
bash
Copy code
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.7.0.96327.zip  
unzip sonarqube-10.7.0.96327.zip  
Start SonarQube:
bash
Copy code
cd sonarqube-10.7.0.96327/bin/linux-x86-64/  
./sonar.sh console  
Allow port 9000 in security groups. Access SonarQube via <public-IP>:9000.
Docker Server Setup
Install Docker using its official documentation.
Verify Docker installation:
bash
Copy code
docker --version  
CI/CD Pipeline Workflow
1. Jenkins Pipeline Configuration
Create a Freestyle Project named Automated-Pipeline.
Connect to the GitHub repository.
Add a GitHub Webhook to trigger builds on code pushes.
2. Integrating SonarQube
Install the SonarQube Scanner plugin in Jenkins.
Add SonarQube Server in Jenkins under Manage Jenkins > System Configuration.
Configure SonarQube scanner in the build step for static code analysis.
3. Deploying with Docker
Add a Dockerfile to the repository:
Dockerfile
Copy code
FROM nginx  
COPY . /usr/share/nginx/html/  
Automate Docker deployment using post-build actions in Jenkins.
Run the Docker container:
bash
Copy code
docker build -t mywebsite .  
docker run -d -p 8085:80 --name Onix-Website mywebsite  
Allow port 8085 in security groups and access the application at <public-IP>:8085.
Pipeline Validation
Trigger a build via GitHub webhook by making changes in the repository.
Validate build status in Jenkins.
Confirm code quality analysis in SonarQube.
Verify successful deployment by accessing the application on the browser.
Future Enhancements
Implement Terraform for infrastructure automation.
Integrate Kubernetes for scalable deployments.

[cicd onix.pdf](https://github.com/user-attachments/files/18024325/cicd.onix.pdf)

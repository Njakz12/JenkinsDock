# Cost-Effective DevOps: Running Jenkins, SonarQube, and Docker on a Single Host
# 1. Set Up the Server
Ensure you have a Linux-based server (e.g., Ubuntu, CentOS, etc.).

Update the system: 
sudo apt update && sudo apt upgrade -y

# 2.  Install Docker
Docker will be used to containerize Jenkins and SonarQube, making it easier to manage and isolate their environments.

Install Docker: sudo apt install docker.io -y
Start and enable Docker: sudo systemctl start docker
sudo systemctl enable docker

# 3.  Grant permissions to $USER to use docker
sudo su - 
usermod -aG docker ubuntu
systemctl restart docker
Logout and login back and try using docker commands without sudo

# 4.  Run Jenkins in a Docker Container
Jenkins can be easily deployed using its official Docker image.
Pull the Jenkins Docker image and Run the Jenkins container:
docker pull jenkins/jenkins:lts
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
 
 Access Jenkins at http://<server-ip>:8080

# 5.  Acess Jenkins
On the Jenkins UI you would have to in put the password
docker exec <container_name_or_id> cat /var/jenkins_home/secrets/initialAdminPassword

# 6.  Run SonarQube in a Docker Container
SonarQube can also be deployed using its official Docker image.
Pull the SonarQube Docker image and Run the SonarQube container:

docker pull sonarqube:lts
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  -v sonarqube_data:/opt/sonarqube/data \
  -v sonarqube_extensions:/opt/sonarqube/extensions \
  sonarqube:lts

# 7.  Grant Jenkins Access to Docker
Jenkins needs access to Docker to build and run containers. Since Jenkins is running in a Docker container, you can bind the Docker socket to the Jenkins container:

docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts

 This allows Jenkins to execute Docker commands on the host.

# 8.  Configure Jenkins and SonarQube Integration
Install the SonarQube Scanner plugin in Jenkins.
Configure Jenkins to connect to SonarQube:

Go to Manage Jenkins > Configure System.

Add SonarQube server details (URL and authentication token).

Use Jenkins pipelines or jobs to run SonarQube scans as part of your CI/CD process.




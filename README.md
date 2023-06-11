# Springboot application pipiline

This pipeline builds, test, create image then push it to DockerHub then modify the Image version in manifest file which triggeres the ArgoCD for the deployment 

## Environment setup

# CI Part
1. Jenkins
    ```
    Run below Jenkins-setup2.sh file from this repo it will install the jenkins
    https://github.com/Sinjith-Reddy/Shell-Script.git 
    ```
2. Maven
    ```
    Here we use Docker agent as worker node which already have Maven installed
    ```
3. SonarQube
    ```
    apt install unzip
    adduser sonarqube
    cd /home/sonarqube/
    wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
    unzip *
    chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
    chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
    cd sonarqube-9.4.0.54424/bin/linux-x86-64/
    su sonarqube
    ./sonar.sh start

    default port is :9000
    default password is admin/admin

    To authenticate with jenkins 
    create a token in sonarQube and add that in jenkins global credentils as secret text type
    ```

# CD part
1. Docker
    ```
    apt-get update
    apt install docker.io
    sudo usermod -aG <group> <user-name>
    sudo usermod -aG docker ubuntu
    sudo usermod -aG docker jenkins
    ```
2. Kubernetes
   ```
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
		sudo dpkg -i minikube_latest_amd64.deb
   ```
3. Kubectl

   -> Update the apt package index and install packages needed to use the Kubernetes apt repository:
    ```
      sudo apt-get update
      sudo apt-get install -y ca-certificates curl
    ```
   -> Download the Google Cloud public signing key:
   ```
     curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
   ```
   -> Add the Kubernetes apt repository:
   ```
      echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
   ```
   -> Update apt package index with the new repository and install kubectl:
   ```
      sudo apt-get update
      sudo apt-get install -y kubectl
   ```
  

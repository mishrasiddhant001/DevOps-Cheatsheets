# ⚙️ **Installation and Setup Guide**

This guide provides a comprehensive step-by-step process for installing and configuring various tools such as **Java**, **Jenkins**, **Node.js**, **Docker**, and more. Follow along for a smooth setup experience! 🚀

---

## ☕ **Install Java (OpenJDK 17)**

1. Update your package index:

   ```bash
   sudo apt update
   ```

2. Install OpenJDK 17:

   ```bash
   sudo apt install openjdk-17-jdk
   ```

3. Verify the Java installation:

   ```bash
   java --version
   ```

4. Update again (just to be thorough 😉):

   ```bash
   sudo apt update
   ```

---

## 🛠️ **Install Jenkins**

1. Download and add the Jenkins key:

   ```bash
   sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   ```

2. Add the Jenkins repository:

   ```bash
   echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
   ```

3. Update the package list:

   ```bash
   sudo apt update
   ```

4. Install Jenkins:

   ```bash
   sudo apt-get install jenkins
   ```

5. Update again (habit 🧑‍💻):

   ```bash
   sudo apt update
   ```

---

## 🔄 **Update and Restart Jenkins After Adding It to \$USER Group**

1. Enable Jenkins:

   ```bash
   sudo systemctl enable jenkins
   ```

2. Start Jenkins:

   ```bash
   sudo systemctl start jenkins
   ```

3. Check Jenkins status:

   ```bash
   sudo systemctl status jenkins
   ```

4. Add your user to the Jenkins group:

   ```bash
   sudo usermod -a -G jenkins $USER
   ```

5. Restart Jenkins to apply changes:

   ```bash
   sudo systemctl restart jenkins
   ```

---

## 🔐 **Jenkins Initial Admin Password**

Retrieve the initial admin password for Jenkins:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## 🔑 **Create SSH Keys for Integration and Authorization**

1. Generate SSH keys:

   ```bash
   ssh-keygen
   ```

2. Navigate to `.ssh` folder:

   ```bash
   cd .ssh
   ```

3. List files:

   ```bash
   ls
   ```

4. View private key:

   ```bash
   cat id_rsa
   ```

5. View public key:

   ```bash
   cat id_rsa.pub
   ```

---

## 🖥️ **Install Node.js, Angular CLI, and NPM**

1. Install unzip utility:

   ```bash
   sudo apt-get install unzip -y
   ```

2. Install **fnm** (Fast Node Manager):

   ```bash
   curl -fsSL https://fnm.vercel.app/install | bash
   ```

3. Apply changes to bashrc:

   ```bash
   source ~/.bashrc
   ```

4. Use the specified version of Node.js:

   ```bash
   fnm use --install-if-missing 22
   ```

5. Verify Node.js and NPM versions:

   ```bash
   node -v
   npm -v
   ```

6. Install Angular CLI:

   ```bash
   npm install -g @angular/cli
   ```

7. Install build essentials:

   ```bash
   sudo apt install build-essential
   ```

8. Install TypeScript:

   ```bash
   npm install -g typescript
   ```

---

## 🌐 **Set-Up Localhost for Testing**

1. Navigate to Jenkins workspace:

   ```bash
   cd /var/lib/jenkins/workspace/rediscover_web_gce
   ```

2. Install dependencies:

   ```bash
   npm install --force
   ```

### 💡 If You Encounter Authorization Error (`EACCESS`)

1. Change ownership of the `.npm` directory:

   ```bash
   sudo chown -R `whoami` ~/.npm
   ```

2. Change ownership of global node modules:

   ```bash
   sudo chown -R `whoami` /usr/local/lib/node_modules
   ```

OR

```bash
sudo chown -R $(whoami) /var/lib/jenkins/workspace/plus\ tv
```

3. Open the web app:

   ```bash
   curl http://34.131.241.78:4200/
   ```

4. Serve the app:

   ```bash
   ng serve --host 0.0.0.0
   ```

---

## 🐳 **Docker Installation**

1. Update package list:

   ```bash
   sudo apt update
   ```

2. Install Docker:

   ```bash
   sudo apt install docker.io
   ```

3. Verify Docker installation:

   ```bash
   docker --version
   ```

4. Check Docker service status:

   ```bash
   sudo systemctl status docker
   ```

---

## 👥 **Add Docker to \$USER Group**

1. Add your user to Docker group:

   ```bash
   sudo usermod -aG docker $USER
   ```

2. Add Jenkins to Docker group:

   ```bash
   sudo usermod -aG docker jenkins
   ```

3. Restart Jenkins:

   ```bash
   sudo systemctl restart jenkins
   ```

---

## 🧹 **Clean Up Docker Images and Containers**

1. Check Docker images:

   ```bash
   sudo docker images
   ```

2. Prune unused Docker objects:

   ```bash
   sudo docker container prune -f \
   && sudo docker image prune -a -f \
   && sudo docker volume prune -f \
   && sudo docker network prune -f \
   && sudo docker system prune -a -f --volumes
   ```

---

## 🧼 **Remove Unnecessary Node Modules Before Creating Dockerfile**

1. Navigate to your project directory:

   ```bash
   cd /var/lib/jenkins/workspace/<directory>
   ```

2. Remove node\_modules:

   ```bash
   sudo rm -rf node_modules
   ```

---

## 📝 **Write Dockerfile**

1. Create or edit Dockerfile:

   ```bash
   sudo vim Dockerfile
   ```

   **Commands**:

   * Press `I` to insert text
   * Press `:wq` to save and quit

---

## 🏗️ **Build and Run Docker Image**

1. Build the Docker image:

   ```bash
   sudo docker build -t docker_images .
   ```

2. Run the Docker image on port 3000:

   ```bash
   sudo docker run -p 3000:80 docker_images
   ```

---

## 🧨 **Remove Running Containers and Images**

1. Clean up Docker containers, images, networks, and volumes:

   ```bash
   sudo docker container prune -f && sudo docker stop (sudo docker ps -aq) && sudo docker rm (sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes
   ```

---

## 🌐 **Send a GET Request to Localhost on Port 3000**

1. Check the app on localhost:

   ```bash
   curl http://localhost:3000/
   ```

---

## 🕵️‍♂️ **Network Exploration and Security Auditing with Nmap**

1. Install **nmap**:

   ```bash
   sudo apt install nmap
   ```

2. Scan port 3000 on a specific IP:

   ```bash
   nmap -p 3000 34.131.241.78
   ```

---

## 💻 **Check Active Listening Ports with Netstat**

1. List all active ports and filter for port 3000:

   ```bash
   netstat -tuln | grep 3000
   ```

---

## 🔥 **Check Firewall Rules with iptables**

1. List firewall rules related to port 3000:

   ```bash
   sudo iptables -L -n -v | grep 3000
   ```

---

## 🔄 **Restart Docker Service**

1. Restart Docker:

   ```bash
   sudo systemctl restart docker
   ```

---

## 🖥️ **Attach to Running Docker Container**

1. Access a running container with ID `857c6b61f13b`:

   ```bash
   sudo docker exec -it 857c6b61f13b /bin/sh
   ```

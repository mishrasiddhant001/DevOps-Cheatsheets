# 1. Install Java
sudo apt install openjdk-17-jdk
java --version
sudo apt update

# 2. Install Jenkins
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt-get install jenkins
sudo apt-get update

# 3. update and restart jenkins after adding it to $USER group
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
sudo usermod -a -G jenkins $USER
sudo systemctl restart jenkins

# 4. Retrieve Jenkins Password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# 5. Create SSH Keys for Integration and Authorization
ssh-keygen
cd .ssh
ls
cat id_rsa
cat id_rsa.pub

# 6. Install Node.js, Angular CLI, and npm
sudo apt-get update
sudo apt install nodejs npm
sudo npm install -g @angular/cli
sudo apt install build-essential
sudo npm install -g typescript
sudo apt-get install unzip -y
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc
fnm use --install-if-missing 22
node -v
npm -v

# 7. Set Up Localhost for Testing
cd /var/lib/jenkins/workspace/rediscover_web_gce
curl http://34.131.241.78:4200/
ng serve --host 0.0.0.0

# 8. Handle Authorization Errors
sudo mkdir /var/lib/jenkins/workspace/<directory>/.angular
sudo chmod -R 777 /var/lib/jenkins/workspace/<directory>/.angular

# 9. Docker Installation
sudo apt update
sudo apt install docker.io
docker --version
sudo systemctl status docker

# 10. Add Docker to $USER Group
sudo usermod -aG docker $USER

# 11. Add Jenkins to Docker Group
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins

# 12. Manage Docker Containers and Images
sudo docker images
sudo docker container prune -f && sudo docker stop $(sudo docker ps -aq) && sudo docker rm $(sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes

# 13. Remove Unnecessary node_modules
cd /var/lib/jenkins/workspace/<directory>
sudo rm -rf node_modules

# 14. Create Dockerfile
cd /var/lib/jenkins/workspace/<directory>
sudo vim Dockerfile
# I - for insert
# :wq - to save and exit

# 15. Build and Run Docker Image
sudo docker build -t docker_images .
sudo docker run -p 3000:80 docker_images

# 16. Clean Up Docker Containers and Images
sudo docker container prune -f && sudo docker stop $(sudo docker ps -aq) && sudo docker rm $(sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes

# 17. Test Docker Container with curl
curl http://localhost:3000/

# 18. Install nmap Utility
sudo apt install nmap

# 19. Scan Port 3000 with nmap
nmap -p 3000 34.131.241.78

# 20. List Active Ports with netstat
netstat -tuln | grep 3000

# 21. Check Firewall Rules with iptables
sudo iptables -L -n -v | grep 3000

# 22. Test Service with curl
curl http://localhost:3000/

# 23. Restart Docker Service
sudo systemctl restart docker

# 24. Attach to Running Docker Container
sudo docker exec -it 857c6b61f13b /bin/sh

# 25. Final Cleanup Before Jenkins CI/CD Automation
sudo docker container prune -f && sudo docker stop $(sudo docker ps -aq) && sudo docker rm $(sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes

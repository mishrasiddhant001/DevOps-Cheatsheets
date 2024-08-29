# Install Java
sudo apt install openjdk-17-jdk

java --version

sudo apt update


# Install Jenkins
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list

sudo apt-get install jenkins

sudo apt-get update 


# Update and restart jenkins after adding it to $USER group
sudo systemctl enable jenkins

sudo systemctl start jenkins

sudo systemctl status jenkins

sudo usermod -a -G jenkin $USER

sudo systemctl restart jenkins


# Password for jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword


# Create ssh keys for intergration and authorization
ssh-keygen

cd .ssh

ls

cat id_rsa

cat id_rsa.pub


# Install nodejs angular-cli and npm
cd 

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


# Set-up localhost for testing 
cd /var/lib/jenkins/workspace/rediscover_web_gce

curl http://34.131.241.78:4200/

ng serve --host 0.0.0.0


# If you get error regarding authorization try these commands
sudo mkdir /var/lib/jenkins/workspace/<directory>/.angular

sudo chmod -R 777 /var/lib/jenkins/workspace/<directory>/.angular


# docker installation
sudo apt update

sudo apt install docker.io

docker --version

sudo systemctl status docker


# add docker to $USER group
sudo usermod -aG docker $USER


# add jenkins to docker group
sudo usermod -aG docker jenkins

sudo systemctl restart jenkins


# Check for running docker images and if unecessary exists
sudo docker images

sudo docker container prune -f && sudo docker stop (sudo docker ps -aq) && sudo docker rm (sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes


# Remove unecessary node_modules before creating Dockerfile from workspace
cd /var/lib/jenkins/workspace/<directory>

sudo rm -rf node_modules


# write the Dockerfile
cd /var/lib/jenkins/workspace/<directory>

sudo vim Dockerfile
{
  Docker commands
}

// I - for insert
// :wq - to save and exit


# build the image and run the image on port 3000 which has been exposed to 80 inside docker and nginx 
sudo docker build -t docker_images .

sudo docker run -p 3000:80 docker_images 


# Remove all the running containers and images along with networks and volumes before going to jenkins CI/CD automation
sudo docker container prune -f && sudo docker stop (sudo docker ps -aq) && sudo docker rm (sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes


# Send a GET request to the localhost on port 3000.
curl http://localhost:3000/


# Install the 'nmap' utility, which is used for network exploration and security auditing.
sudo apt install nmap


# Use 'nmap' to scan port 3000 on the IP address 34.131.241.78 to check if the port is open.
nmap -p 3000 34.131.241.78


# Use 'netstat' to list all active listening and non-listening (server/client) ports and filter the output to show only those related to port 3000.
netstat -tuln | grep 3000


# Use 'iptables' to list all the firewall rules and filter the output for entries related to port 3000.
sudo iptables -L -n -v | grep 3000


# Send another GET request to the localhost on port 3000. This is likely done to check if the service is up after performing the previous steps.
curl http://localhost:3000/


# Restart the Docker service. This command is often used when containers or Docker services need to be refreshed.
sudo systemctl restart docker


# Attach to the running Docker container with ID 857c6b61f13b and open an interactive shell session within it.
sudo docker exec -it 857c6b61f13b /bin/sh


# Remove all the running containers and images along with networks and volumes before going to jenkins CI/CD automation
sudo docker container prune -f && sudo docker stop $(sudo docker ps -aq) && sudo docker rm $(sudo docker ps -aq) && sudo docker image prune -a -f && sudo docker volume prune -f && sudo docker network prune -f && sudo docker system prune -a -f --volumes

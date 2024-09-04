#!/bin/bash

## Exit script on any error
set -e

## Define variables
DOMAIN="www.website.com"
CERT_PATH="/etc/letsencrypt/live/${DOMAIN}"
PROJECT_DIR="/var/lib/jenkins/workspace/Project_directory"
NGINX_SSL_DIR="${PROJECT_DIR}/nginx/ssl"

## Update system and install necessary packages
sudo apt update
sudo apt install -y certbot python3-certbot-nginx openssl docker.io

## Generate Let's Encrypt Certificates (Uncomment if needed)
# sudo certbot certonly --standalone -d ${DOMAIN} -d admin.${DOMAIN}

## Verify Let's Encrypt Certificates
# sudo ls ${CERT_PATH}

## Create project directory for SSL certificates
mkdir -p ${NGINX_SSL_DIR}

## Generate self-signed certificates and DH parameters
sudo openssl req -newkey rsa:2048 -nodes -keyout ${NGINX_SSL_DIR}/nginx-selfsigned.key -x509 -days 365 -out ${NGINX_SSL_DIR}/nginx-selfsigned.crt
sudo openssl dhparam -out ${NGINX_SSL_DIR}/dhparam.pem 2048

## Set appropriate permissions for the SSL certificates
sudo chmod 644 ${NGINX_SSL_DIR}/nginx-selfsigned.key
sudo chmod 644 ${NGINX_SSL_DIR}/nginx-selfsigned.crt
sudo chmod 644 ${NGINX_SSL_DIR}/dhparam.pem

## Create Dockerfile
cat <<EOF > ${PROJECT_DIR}/Dockerfile
## Stage 1: Build the Angular application
FROM node:16-alpine3.11 AS build

WORKDIR /app

COPY package*.json ./
RUN npm install --force
COPY . .
RUN npm run build -- --configuration production

## Stage 2: Serve the Angular application using Nginx
FROM nginx:alpine

COPY nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/rediscover-ui /usr/share/nginx/html
COPY nginx/ssl /etc/nginx/ssl

EXPOSE 443
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
EOF

## Create Nginx configuration
cat <<EOF > ${PROJECT_DIR}/nginx/default.conf
## Redirect HTTP to HTTPS
server {
    listen 80;
    server_name 34.131.70.205;

    location / {
        return 301 https://\$host\$request_uri;
    }
}

## Main HTTPS server block
server {
    listen 443 ssl;
    server_name 34.131.70.205;

    ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:!aNULL:!MD5:!3DES';
    ssl_prefer_server_ciphers on;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer" always;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files \$uri \$uri/ =404;
    }

    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        try_files \$uri =404;
        expires 30d;
        access_log off;
    }

    location ~* \.(conf|git|svn|env|sh)$ {
        deny all;
    }
}
EOF

## Build Docker image
cd ${PROJECT_DIR}
docker build -t your-angular-app .

## Run Docker container
docker run -d -p 443:443 your-angular-app

echo "Setup complete. Your Angular app is running on port 443."

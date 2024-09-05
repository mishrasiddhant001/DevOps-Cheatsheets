cat <<EOF>
server {
    server_name  dmoain.com www.dmoain.com;

    root /var/www/html/porject_directory/"outputPath": "dist/_____________";

    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    error_page 404 /index.html;

    location ~ /\.ht {
        deny all;
    }

    # Optional: Configure gzip compression
    gzip on;
    gzip_types text/plain text/css application/javascript application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_proxied any;
    gzip_vary on;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_min_length 256;

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/www.dmoain.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/www.dmoain.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
    server_name dmoain.com;

    # Redirect all traffic from non-www to www
    return 301 https://www.dmoain.com$request_uri;
    
    listen 80;
    listen [::]:80;
}
EOF

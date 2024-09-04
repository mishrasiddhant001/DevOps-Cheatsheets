## Ngnix/default.conf for a website

cat<<EOF>server {
    server_name www.vizzytv.com;

    root /var/www/html/ReDiscover-WEB/dist/rediscover-ui;

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
    ssl_certificate /etc/letsencrypt/live/www.vizzytv.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/www.vizzytv.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
    server_name vizzytv.com;

    # Redirect all traffic from non-www to www
    return 301 https://www.vizzytv.com$request_uri;
    
    listen 80;
    listen [::]:80;
}
EOF

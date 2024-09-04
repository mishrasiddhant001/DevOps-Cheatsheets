## Nginx Configuration for a Website

Below is the `nginx/default.conf` configuration for the website `www.vizzytv.com`. This configuration sets up SSL, handles gzip compression, and redirects non-www traffic to the www version.

### Nginx Configuration File

```nginx
# HTTPS server block for www.website.com
server {
    server_name www.vizzytv.com;

    root /var/www/html/ReDiscover-WEB/dist/rediscover-ui;
    index index.html;

    # Handle requests and fall back to index.html for SPA routing
    location / {
        try_files $uri $uri/ /index.html;
    }

    # Custom error page for 404 errors
    error_page 404 /index.html;

    # Deny access to hidden files
    location ~ /\.ht {
        deny all;
    }

    # Optional: Configure gzip compression for better performance
    gzip on;
    gzip_types text/plain text/css application/javascript application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_proxied any;
    gzip_vary on;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_min_length 256;

    # SSL configuration (managed by Certbot)
    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/www.website.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/www.website.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
}

# Redirect HTTP traffic from non-www to www
server {
    server_name website.com;

    return 301 https://www.website.com$request_uri;
    
    listen 80;
    listen [::]:80;
}

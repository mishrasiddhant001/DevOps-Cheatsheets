### Nginx Server Configuration for **Admin** 🚀

This guide walks you through setting up an Nginx configuration file for **Admin Admin**, ensuring proper redirection from HTTP to HTTPS, SSL certificates setup, and PHP-FPM handling. Let's break it down! 👇

---

#### 1. **HTTP to HTTPS Redirection 🌐🔒**

The first server block redirects all incoming HTTP requests to HTTPS to ensure a secure connection.

```nginx
server {
    listen 80; 
    server_name site.com;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}
```

* **`listen 80;`** – Listen on port 80 (HTTP).
* **`return 301`** – Permanently redirect HTTP to HTTPS.

---

#### 2. **HTTPS Server Configuration 🔒💻**

Now, we configure the server to listen on HTTPS (port 443) and apply the SSL certificates. Here's where we set up security! 🔐

```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name site.com;

    ssl_certificate /etc/letsencrypt/live/site.com/fullchain.pem; # SSL Certificate from Certbot
    ssl_certificate_key /etc/letsencrypt/live/site.com/privkey.pem; # SSL Key from Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    root /var/www/html/Admin/public;  # Website Root Directory
    index index.php index.html index.htm;

    # PHPMyAdmin Configuration (optional)
    include snippets/phpmyadmin.conf;
}
```

* **SSL Certificates**: The **Certbot** tool manages your SSL certificates.
* **SSL Params**: Ensures secure Diffie-Hellman parameters are used for encryption.
* **Root Directory**: Specifies where your website files are located (`/var/www/html/Admin/public`).
* **PHPMyAdmin**: If you want PHPMyAdmin access, include the snippet.

---

#### 3. **Handling Requests and PHP Files 📂⚙️**

We now set up how requests are handled, including how to deal with PHP files.

```nginx
# Main site location: Ensures URL rewriting and access to PHP
location / {
    try_files $uri $uri/ /index.php?q=$uri&$args;
    fastcgi_read_timeout 3600s;  # Extend PHP script timeout
    fastcgi_send_timeout 3600s;  # Extend PHP script send timeout
}

# PHP File Processing
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php8.2-fpm.sock;  # Connects to PHP-FPM
    fastcgi_read_timeout 3600s;
    fastcgi_send_timeout 3600s;
}

# Security: Deny access to hidden files
location ~ /\.ht {
    deny all;
}
```

* **`try_files`**: Tries to serve the file if it exists or reroutes to `index.php`.
* **`fastcgi_pass`**: Connects Nginx to PHP-FPM to process PHP requests.
* **Security**: Denies access to hidden files, such as `.htaccess`, for security.

---

### **Full Code for Nginx Configuration** 📜

```nginx
server {
    listen 80; 
    server_name site.com;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name site.com;

    ssl_certificate /etc/letsencrypt/live/site.com/fullchain.pem; # SSL Certificate from Certbot
    ssl_certificate_key /etc/letsencrypt/live/site.com/privkey.pem; # SSL Key from Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    root /var/www/html/Admin/public;  # Website Root Directory
    index index.php index.html index.htm;

    # PHPMyAdmin Configuration (optional)
    include snippets/phpmyadmin.conf;

    # Main site location: Ensures URL rewriting and access to PHP
    location / {
        try_files $uri $uri/ /index.php?q=$uri&$args;
        fastcgi_read_timeout 3600s;
        fastcgi_send_timeout 3600s;
    }

    # PHP File Processing
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;  # Connects to PHP-FPM
        fastcgi_read_timeout 3600s;
        fastcgi_send_timeout 3600s;
    }

    # Security: Deny access to hidden files
    location ~ /\.ht {
        deny all;
    }
}
```

---
Full Code for Nginx Configuration 📜
```
server {
    listen 80; 
    server_name site.com;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name site.com;

    ssl_certificate /etc/letsencrypt/live/site.com/fullchain.pem; # SSL Certificate from Certbot
    ssl_certificate_key /etc/letsencrypt/live/site.com/privkey.pem; # SSL Key from Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    root /var/www/html/Admin/public;  # Website Root Directory
    index index.php index.html index.htm;

    # PHPMyAdmin Configuration (optional)
    include snippets/phpmyadmin.conf;

    # Main site location: Ensures URL rewriting and access to PHP
    location / {
        try_files $uri $uri/ /index.php?q=$uri&$args;
        fastcgi_read_timeout 3600s;
        fastcgi_send_timeout 3600s;
    }

    # PHP File Processing
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;  # Connects to PHP-FPM
        fastcgi_read_timeout 3600s;
        fastcgi_send_timeout 3600s;
    }

    # Security: Deny access to hidden files
    location ~ /\.ht {
        deny all;
    }
}
```
### 🚀 **What's Next?**

1. **Test the configuration** using `nginx -t` to ensure there are no errors.
2. **Reload Nginx** with `systemctl reload nginx` for the changes to take effect.

This setup ensures a secure, fast, and efficient environment for **Admin**! ✨


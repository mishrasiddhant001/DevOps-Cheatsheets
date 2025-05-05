# Nginx Configuration for `admin.com` 🔧🌐

This document provides a detailed explanation of the Nginx configuration server. The configuration is designed to serve a web application with PHP support, enforce HTTPS for secure communication, and include access to phpMyAdmin. It consists of two `server` blocks: one for redirecting HTTP traffic to HTTPS and another for handling HTTPS requests.

---

## 📚 Table of Contents

1. [Overview](#overview)
2. [HTTP to HTTPS Redirect](#http-to-https-redirect)
3. [HTTPS Server Configuration](#https-server-configuration)

   * [SSL/TLS Settings](#ssltls-settings)
   * [Document Root and Index Files](#document-root-and-index-files)
   * [Included Snippets](#included-snippets)
4. [Location Blocks](#location-blocks)

   * [Root Location (`/`): Handling General Requests](#root-location--handling-general-requests)
   * [PHP Files Location (`~ \.php$`): Processing PHP Requests](#php-files-location--processing-php-requests)
   * [Deny .ht Files Location (`~ /\.ht`): Enhancing Security](#deny-ht-files-location--enhancing-security)
   * [phpMyAdmin Location (`/phpmyadmin`): Accessing phpMyAdmin](#phpmyadmin-location-phpmyadmin-accessing-phpmyadmin)
5. [🔧 Additional Notes](#additional-notes)

---

## 🧐 Overview

The configuration includes two main `server` blocks:

* **HTTP Server Block**: Listens on port 80 and redirects all traffic to HTTPS, ensuring secure communication.
* **HTTPS Server Block**: Listens on port 443, serves the web application from `/var/www/html/MurMur/public`, processes PHP files via FastCGI, and provides access to phpMyAdmin.

This setup ensures that all traffic is encrypted and provides a secure environment for hosting the application.

---

## 🔄 HTTP to HTTPS Redirect

```nginx
server {
    listen 80;
    server_name admin.com;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}
```

### 📌 Explanation

* **`listen 80`**: Configures the server to listen on port 80, the standard port for HTTP traffic.
* **`server_name admin.com`**: Specifies that this block applies to requests for the domain `admin.com`.
* **`return 301 https://$host$request_uri`**: Issues a **301** (permanent) redirect to the HTTPS version of the requested URL. The `$host` variable preserves the domain name, and `$request_uri` appends the original request path and query string. This forces HTTPS for all traffic.

---

## 🔒 HTTPS Server Configuration

```nginx
server {
    listen 443 ssl;
    server_name admin.com;

    ssl_certificate /path/to/fullchain.pem;
    ssl_certificate_key /path/to/privkey.pem;

    root /var/www/html/MurMur/public;
    index index.php;

    include snippets/fastcgi-php.conf;
    include snippets/ssl-params.conf;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;  # Adjust based on PHP version
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }

    location /phpmyadmin {
        alias /usr/share/phpmyadmin;
        index index.php;
        try_files $uri $uri/ /phpmyadmin/index.php;
    }
}
```

### 🔑 SSL/TLS Settings

* **`listen 443 ssl`**: Configures the server to listen on port 443 (standard HTTPS port) with SSL enabled for encryption.
* **`ssl_certificate /path/to/fullchain.pem`**: Specifies the path to the SSL certificate file, which includes the server certificate and any intermediate certificates (full chain).
* **`ssl_certificate_key /path/to/privkey.pem`**: Specifies the path to the private key corresponding to the certificate. These settings enable secure HTTPS connections.

### 📂 Document Root and Index Files

* **`root /var/www/html/MurMur/public`**: Sets the document root to `/var/www/html/MurMur/public`, where the web application’s public files are stored.
* **`index index.php`**: Defines `index.php` as the default file to serve when a directory is requested, typical for PHP-based applications.

### ⚙️ Included Snippets

* **`include snippets/fastcgi-php.conf`**: Imports a configuration file with settings for processing PHP requests via FastCGI, streamlining PHP handling.
* **`include snippets/ssl-params.conf`**: Imports additional SSL/TLS settings (e.g., cipher suites, protocols) to enhance security beyond the basic certificate configuration.

---

## 🔄 Location Blocks

### 🏠 Root Location (`/`): Handling General Requests

```nginx
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

* **Purpose**: Manages requests to the root URL and its subdirectories.
* **`try_files $uri $uri/ /index.php?$query_string`**: Attempts to serve:

  1. The exact file matching the request URI (`$uri`).
  2. A directory matching the URI (`$uri/`).
  3. If neither is found, passes the request to `index.php` with the original query string (`$query_string`). This is a common setup for PHP applications using a front controller pattern.

### 🖥️ PHP Files Location (`~ \.php$`): Processing PHP Requests

```nginx
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;  # Adjust based on PHP version
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
}
```

* **Purpose**: Handles requests for files ending in `.php`.
* **`include snippets/fastcgi-php.conf`**: Reuses the FastCGI settings for PHP processing.
* **`fastcgi_pass unix:/var/run/php/php7.4-fpm.sock`**: Forwards PHP requests to the PHP-FPM service via a Unix socket (adjust the socket path based on the installed PHP version, e.g., `php8.0-fpm.sock`).
* **`fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name`**: Specifies the full path to the PHP script to execute, ensuring PHP-FPM processes the correct file.

### 🚫 Deny .ht Files Location (`~ /\.ht`): Enhancing Security

```nginx
location ~ /\.ht {
    deny all;
}
```

* **Purpose**: Prevents access to files starting with `.ht` (e.g., `.htaccess`, `.htpasswd`), which are typically Apache configuration files.
* **`deny all`**: Blocks all requests to these files, enhancing security on an Nginx server where such files are irrelevant.

### 🔑 phpMyAdmin Location (`/phpmyadmin`): Accessing phpMyAdmin

```nginx
location /phpmyadmin {
    alias /usr/share/phpmyadmin;
    index index.php;
    try_files $uri $uri/ /phpmyadmin/index.php;
}
```

* **Purpose**: Provides access to phpMyAdmin, a web-based database management tool, at the `/phpmyadmin` URL.
* **`alias /usr/share/phpmyadmin`**: Maps requests to the phpMyAdmin installation directory (adjust the path if phpMyAdmin is installed elsewhere).
* **`index index.php`**: Sets `index.php` as the default file for the `/phpmyadmin` directory.
* **`try_files $uri $uri/ /phpmyadmin/index.php`**: Attempts to serve the requested file or directory, falling back to `index.php` within phpMyAdmin if not found.

# Nginx Configuration for phpMyAdmin 🛠️

This document describes an Nginx configuration snippet located at `/etc/nginx/snippets/phpmyadmin.conf`. It is designed to serve **phpMyAdmin**, a web-based tool for managing MySQL databases, ensuring that your phpMyAdmin instance is correctly served and secured.

---

## Configuration Snippet

```nginx
location /phpmyadmin {
    root /usr/share/;
    index index.php index.html index.htm;

    # PHP Handling
    location ~ ^/phpmyadmin/(.+\.php)$ {
        try_files $uri =404;
        root /usr/share/;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;  # Adjust based on your PHP version
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include /etc/nginx/fastcgi_params;
    }

    # Static Files (images, CSS, JS)
    location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
        root /usr/share/;
    }
}
```

---

## 📌 Explanation

* **`location /phpmyadmin`**: This is the main block for handling requests to `/phpmyadmin`, the URL path for accessing phpMyAdmin.

  * **`root /usr/share/;`**: Specifies the directory where phpMyAdmin is located. This assumes phpMyAdmin is installed in `/usr/share/phpmyadmin`.
  * **`index index.php index.html index.htm;`**: Sets the default index files to look for when a directory is accessed.

* **PHP Files Handling (`~ ^/phpmyadmin/(.+\.php)$`)**:

  * **`try_files $uri =404;`**: Attempts to serve the requested PHP file; if not found, returns a 404 error.
  * **`fastcgi_pass unix:/run/php/php8.2-fpm.sock;`**: Forwards PHP requests to the PHP-FPM service via a Unix socket (make sure to adjust this based on your PHP version).
  * **`fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;`**: Ensures PHP-FPM processes the correct PHP script file.
  * **`include /etc/nginx/fastcgi_params;`**: Includes Nginx's default FastCGI parameters to handle PHP processing correctly.

* **Static Files Handling (`~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$`)**:

  * **`root /usr/share/;`**: Ensures that static assets like images, CSS, JS, etc., are served correctly from the phpMyAdmin directory.
  * **`location ~*`**: This ensures the matching of file extensions such as `.jpg`, `.jpeg`, `.gif`, `.css`, `.png`, `.js`, `.ico`, `.html`, `.xml`, `.txt`, etc.


---

## 🔧 Additional Notes

* **SSL Paths**: Replace `/path/to/fullchain.pem` and `/path/to/privkey.pem` with the actual paths to your SSL certificate and key files.
* **PHP Version**: Adjust the `fastcgi_pass` socket (e.g., `php7.4-fpm.sock`) to match your installed PHP-FPM version.
* **phpMyAdmin Path**: Verify that `/usr/share/phpmyadmin` matches your server’s phpMyAdmin installation directory; adjust if necessary.
* **Security Enhancements**: For production, consider adding:

  * IP whitelisting or basic authentication for `/phpmyadmin` to restrict access.
  * Updated SSL parameters in `snippets/ssl-params.conf` to enforce modern security standards.

This configuration ensures that **admin.com** securely serves a PHP-based web application over HTTPS, with proper PHP processing and access to phpMyAdmin for database administration.

* **PHP Version**: Make sure to adjust the PHP-FPM socket path (e.g., `php8.2-fpm.sock`) based on your installed PHP version.
* **Security**: Consider restricting access to phpMyAdmin by IP address or using additional authentication to secure it, especially if your server is publicly accessible.

This configuration ensures that phpMyAdmin is served correctly, with support for PHP processing, static file handling, and security for web-based database management.


```
ubuntu@ip-172-31-44-119:/etc/nginx$ cat sites-enabled/murmur-admin.conf
server {
    listen 80;
    server_name admin.com;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name admin.com;

    ssl_certificate /etc/letsencrypt/live/admin.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/admin.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;


    root /var/www/html/MurMur/public;
    index index.php index.html index.htm;

    include snippets/phpmyadmin.conf;
#    include snippets/jenkins.conf;
#    access_log /var/log/nginx/jenkins.access.log;
#    error_log /var/log/nginx/jenkins.error.log;

    location / {
        try_files $uri $uri/ /index.php?q=$uri&$args;
        fastcgi_read_timeout 3600s;
        fastcgi_send_timeout 3600s;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
        fastcgi_read_timeout 3600s;
        fastcgi_send_timeout 3600s;
    }
    location ~ /\.ht {
        deny all;
    }
}
```

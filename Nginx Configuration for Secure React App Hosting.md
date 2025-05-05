# 🌐🔐 Nginx Configuration for Secure React App Hosting

This Nginx configuration securely serves a **React Single Page Application (SPA)** over HTTPS using **Let's Encrypt SSL**. It includes best practices for **security headers**, **static file caching**, and **routing**.

---

## 🧰 1. HTTPS Server Block (`443` with SSL)

```nginx
server {
    listen 443 ssl http2;
    server_name site.tvidservices.com www.site.tvidservices.com;
```

### ✅ What it does:

* ✅ **Listens on port 443** for HTTPS traffic.
* 🔒 **Enables SSL** and modern **HTTP/2** protocol.
* 🌍 Serves traffic for:

  * `site.tvidservices.com`
  * `www.site.tvidservices.com`

---

## 🔐 2. SSL Certificate Configuration

```nginx
    ssl_certificate /etc/letsencrypt/live/site.tvidservices.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/site.tvidservices.com/privkey.pem;
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;
```

### 🧾 Explanation:

* 📜 `fullchain.pem`: Certificate chain (includes intermediate and root).
* 🔑 `privkey.pem`: Your private key (keep it secure!).
* 🧮 `dhparam.pem`: Enables strong encryption via **Diffie-Hellman key exchange**.

---

## 🔒 3. SSL Protocols & Cipher Settings

```nginx
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:!aNULL:!MD5:!3DES';
    ssl_prefer_server_ciphers on;
```

### 🚀 Security Setup:

* ✅ Supports **TLS 1.2 and 1.3** (modern and secure).
* 🔐 Uses **strong cipher suites** for encryption.
* 🎛️ Prefers server-defined ciphers over client-suggested ones.

---

## 🛡️ 4. Security Headers (Best Practices)

```nginx
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer" always;
```

### 🧯 Purpose:

* 📅 `Strict-Transport-Security`: Enforces HTTPS for 1 year.
* 🧪 `X-Content-Type-Options`: Prevents MIME-type sniffing.
* 🖼️ `X-Frame-Options`: Prevents **clickjacking** by allowing framing only from the same origin.
* 🛡️ `X-XSS-Protection`: Enables browser's XSS protection.
* 🕵️‍♂️ `Referrer-Policy`: Keeps referer info private.

---

## 📂 5. Root Directory & React Routing

```nginx
    root /var/www/html/site-React/build;
    index index.html;
```

### 🏠 React App Setup:

* 🧱 Root is the `build` directory from your React app.
* 🧭 Default entry point is `index.html`.

---

## 🧭 6. React SPA Routing (Single Page App)

```nginx
    location / {
        try_files $uri $uri/ /index.html;
    }
```

### 🔁 Why?

* Ensures all **non-static routes** return `index.html`.
* Critical for **React Router** or similar front-end routing libraries.

---

## ⚡ 7. Static Files & Caching

```nginx
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|woff|woff2|ttf|svg|eot)$ {
        try_files $uri =404;
        expires 30d;
        access_log off;
    }
```

### 📦 Optimization:

* ✅ Serves images, fonts, CSS, JS, etc.
* 🕒 Cache expiration: `30 days` (reduces bandwidth and speeds up load).
* 📉 Turns off logging to reduce file size and noise.

---

## 🚫 8. Block Sensitive Files

```nginx
    location ~* \.(conf|git|svn|env|sh)$ {
        deny all;
    }
```

### 🔐 Protects:

* Configuration files (`.conf`)
* Git or SVN metadata (`.git`, `.svn`)
* Shell/environment files (`.env`, `.sh`)
* ❌ All are denied from public access.

---

## 🔁 9. HTTP to HTTPS Redirect (`80 ➡️ 443`)

```nginx
server {
    if ($host ~* ^(www\.)?site.tvidservices.com$) {
        return 301 https://$host$request_uri;
    }

    listen 80;
    server_name site.tvidservices.com www.site.tvidservices.com;

    location / {
        return 301 https://$host$request_uri;
    }
}
```

### 🧭 Redirects:

* All requests on **port 80 (HTTP)** get **301 redirected** to **HTTPS (port 443)**.
* Ensures all traffic is **encrypted**.
* Good for **SEO** and **security**.

---

## ✅ Summary Table

| 🔧 Feature                 | ✅ Enabled |
| -------------------------- | --------- |
| HTTPS (SSL)                | ✅         |
| HTTP/2                     | ✅         |
| Auto Redirect HTTP → HTTPS | ✅         |
| Let's Encrypt Certificates | ✅         |
| Strong SSL Ciphers         | ✅         |
| Security Headers           | ✅         |
| React SPA Routing          | ✅         |
| Static File Caching        | ✅         |
| Sensitive File Protection  | ✅         |

```
# Main HTTPS server block
server {
    listen 443 ssl http2;
    server_name site.tvidservices.com www.site.tvidservices.com;

    # SSL certificate files from Certbot
    ssl_certificate /etc/letsencrypt/live/site.tvidservices.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/site.tvidservices.com/privkey.pem; # managed by Certbot
    ssl_dhparam /etc/nginx/ssl/dhparam.pem;

    # SSL protocols and ciphers
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:!aNULL:!MD5:!3DES';
    ssl_prefer_server_ciphers on;

    # Security headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "no-referrer" always;

    # Root directory and index file
    root /var/www/html/site-React/build;
    index index.html;

    # React SPA routing
    location / {
        try_files $uri $uri/ /index.html;
    }

    # Static file caching
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|woff|woff2|ttf|svg|eot)$ {
        try_files $uri =404;
        expires 30d;
        access_log off;
    }

    # Deny access to sensitive files
    location ~* \.(conf|git|svn|env|sh)$ {
        deny all;
    }
}

# Redirect HTTP to HTTPS
server {
    if ($host ~* ^(www\.)?site.tvidservices.com$) {
        return 301 https://$host$request_uri;
    }

    listen 80;
    server_name site.tvidservices.com www.site.tvidservices.com;

    location / {
        return 301 https://$host$request_uri;
    }
}
```

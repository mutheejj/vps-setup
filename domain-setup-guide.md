# Domain & Subdomain Setup Guide

## Main Domain Setup

### For Normal Projects

**1. DNS Setup**
```
Type: A
Host: @ (or main.com)
Answer: your-server-ip
TTL: 300
```

**2. Create Directory**
```bash
sudo mkdir -p /var/www/main.com
echo "<h1>Main Site</h1>" | sudo tee /var/www/main.com/index.html
```

**3. Nginx Config**
```bash
sudo nano /etc/nginx/sites-available/main.com
```

```nginx
server {
    listen 80;
    server_name main.com www.main.com;
    root /var/www/main.com;
    index index.html;
}
```

**4. Enable Site**
```bash
sudo ln -s /etc/nginx/sites-available/main.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

**5. SSL**
```bash
sudo certbot --nginx -d main.com -d www.main.com
```

---

### For Laravel Projects

**1. DNS Setup** (Same as Normal)
```
Type: A
Host: @ (or main.com)
Answer: your-server-ip
TTL: 300
```

**2. Create Directory**
```bash
sudo mkdir -p /var/www/main.com
# Upload Laravel files to /var/www/main.com
sudo chown -R www-data:www-data /var/www/main.com
sudo chmod -R 755 /var/www/main.com/storage
```

**3. Nginx Config**
```bash
sudo nano /etc/nginx/sites-available/main.com
```

```nginx
server {
    listen 80;
    server_name main.com www.main.com;
    root /var/www/main.com/public;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}
```

**4. Enable Site** (Same as Normal)
**5. SSL** (Same as Normal)

---

## Subdomain Setup

### For Normal Projects

**1. DNS Setup**
```
Type: A
Host: subdomain (example: api, blog)
Answer: your-server-ip
TTL: 300
```

**2. Create Directory**
```bash
sudo mkdir -p /var/www/subdomain.main.com
echo "<h1>Subdomain Site</h1>" | sudo tee /var/www/subdomain.main.com/index.html
```

**3. Nginx Config**
```bash
sudo nano /etc/nginx/sites-available/subdomain.main.com
```

```nginx
server {
    listen 80;
    server_name subdomain.main.com;
    root /var/www/subdomain.main.com;
    index index.html;
}
```

**4. Enable Site**
```bash
sudo ln -s /etc/nginx/sites-available/subdomain.main.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

**5. SSL**
```bash
sudo certbot --nginx -d subdomain.main.com
```

---

### For Laravel Projects

**1. DNS Setup** (Same as Normal)
```
Type: A
Host: subdomain
Answer: your-server-ip
TTL: 300
```

**2. Create Directory**
```bash
sudo mkdir -p /var/www/subdomain.main.com
# Upload Laravel files to /var/www/subdomain.main.com
sudo chown -R www-data:www-data /var/www/subdomain.main.com
sudo chmod -R 755 /var/www/subdomain.main.com/storage
```

**3. Nginx Config**
```bash
sudo nano /etc/nginx/sites-available/subdomain.main.com
```

```nginx
server {
    listen 80;
    server_name subdomain.main.com;
    root /var/www/subdomain.main.com/public;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}
```

**4. Enable Site** (Same as Normal)
**5. SSL** (Same as Normal)

---

## Quick Commands

**Install Certbot (if needed)**
```bash
sudo apt install certbot python3-certbot-nginx -y
```

**Test Nginx Config**
```bash
sudo nginx -t
```

**Restart Nginx**
```bash
sudo systemctl restart nginx
```

**Verify Sites**
Visit: https://main.com or https://subdomain.main.com

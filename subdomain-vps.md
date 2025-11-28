# How to Connect a Subdomain to a VPS and Host Multiple Sites on One Server

This guide explains how to point a subdomain from Name.com to a VPS and map each domain/subdomain to a different directory on the server so multiple sites can run independently.

---

## 1. Create Subdomain on Name.com

### **Step 1: Open DNS Settings**

1. Log into Name.com.
2. Go to **My Domains** → Select your domain.
3. Click **DNS Records**.

### **Step 2: Add Subdomain A Record**

```
Type: A
Host: subdomain   (example: api, blog, app)
Answer: your-server-ip
TTL: 300
```

Example:

```
Host: api
Answer: 143.110.22.10
```

---

## 2. Create a Directory for Each Site (Different Locations)

Create a folder for each domain/subdomain:

```bash
sudo mkdir -p /var/www/main.com
sudo mkdir -p /var/www/api.main.com
sudo mkdir -p /var/www/blog.main.com
```

Add a test file:

```bash
echo "<h1>Main Site</h1>" | sudo tee /var/www/main.com/index.html
echo "<h1>API Site</h1>" | sudo tee /var/www/api.main.com/index.html
echo "<h1>Blog Site</h1>" | sudo tee /var/www/blog.main.com/index.html
```

---

## 3. Create Nginx Config for Each Site

### **Main Domain**

```bash
sudo nano /etc/nginx/sites-available/main.com
```

Paste:

```nginx
server {
    listen 80;
    server_name main.com www.main.com;
    root /var/www/main.com;
    index index.html;
}
```

### **Subdomain: api.main.com**

```bash
sudo nano /etc/nginx/sites-available/api.main.com
```

Paste:

```nginx
server {
    listen 80;
    server_name api.main.com;
    root /var/www/api.main.com;
    index index.html;
}
```

### **Subdomain: blog.main.com**

```bash
sudo nano /etc/nginx/sites-available/blog.main.com
```

Paste:

```nginx
server {
    listen 80;
    server_name blog.main.com;
    root /var/www/blog.main.com;
    index index.html;
}
```

---

## 4. Enable the Sites

Create symlinks:

```bash
sudo ln -s /etc/nginx/sites-available/main.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/api.main.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/blog.main.com /etc/nginx/sites-enabled/
```

Test config:

```bash
sudo nginx -t
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## 5. Enable SSL for Each Domain/Subdomain

Install Certbot (if not installed):

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Run SSL setup:

```bash
sudo certbot --nginx -d main.com -d www.main.com
sudo certbot --nginx -d api.main.com
sudo certbot --nginx -d blog.main.com
```

Choose **Redirect** when prompted.

---

## 6. Verify Sites

Visit:

```
https://main.com
https://api.main.com
https://blog.main.com
```

Each subdomain now loads from its own directory.

---

## Done

You now have:

* Multiple sites running on one VPS.
* Each domain/subdomain mapped to a different folder.
* Full SSL support for all.


# BPA Server Setup Guide

This guide shows the minimal steps to connect a domain to a VPS, install Nginx, enable SSL, and access the default Nginx welcome page.

---

## 1. Connect Domain to VPS (Name.com → DigitalOcean)

### **Step 1: Add Domain to DigitalOcean**

1. Open DigitalOcean → **Networking** → **Domains**.
2. Click **Add Domain**.
3. Enter your domain (e.g., `example.com`).

### **Step 2: Update Nameservers on Name.com**

Set these nameservers:

```
ns1.digitalocean.com
ns2.digitalocean.com
ns3.digitalocean.com
```

### **Step 3: Add A Records on DigitalOcean**

Inside **Networking → Domains → example.com**, add:

```
A   @      your-droplet-ip
A   www    your-droplet-ip
```

---

## 2. Install Nginx on Ubuntu VPS

Run:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
```

Check status:

```bash
systemctl status nginx
```

After installation, visiting your domain will show the **default Nginx welcome page**.

---

## 3. Enable SSL (HTTPS)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d example.com -d www.example.com
```

Choose **Redirect** when prompted.

---

## 4. Access Your Server

Visit:

```
http://example.com
https://example.com
```

You should see the default Nginx welcome page.


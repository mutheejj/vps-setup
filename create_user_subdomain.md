# How to Create a New User (Subdomain Instance) Using the Same Laravel Codebase

This guide explains how to host another project instance on a **subdomain** using the **same Laravel codebase**, by creating a **separate virtual host**.

---

## ✅ METHOD 1: Use the SAME CODE but Create a Separate VIRTUAL HOST

This is the cleanest and most common method.  
You simply point the new subdomain to the **same Laravel `/public` directory**.

---

## ✔ Step 1: Create the Subdomain in Your DNS Panel

Create a DNS A record (Cloudflare, cPanel, Hostinger, Namecheap, etc.):

| Field | Value |
|-------|--------|
| **Type** | A |
| **Name** | `john` |
| **Value** | *your-server-ip* |
| **TTL** | Auto |

This makes:

```
john.mas.codes → your server
```

---

## ✔ Step 2: Create a New NGINX Virtual Host

Create a site file:

```bash
sudo nano /etc/nginx/sites-available/john.mas.codes
```

Paste this:

```nginx
server {
    listen 80;
    server_name john.mas.codes;

    root /var/www/portfolio/public;
    index index.php index.html;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

Save the file.

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/john.mas.codes /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

---

## ✔ Step 3: Update `.env` (OPTIONAL)

If you want the subdomain to have its own URL:

```
APP_URL=https://john.mas.codes
```

If the same website serves all domains, leave `.env` unchanged.

---

## ✔ Step 4: Issue an SSL Certificate

Run:

```bash
sudo certbot --nginx -d john.mas.codes
```

This automatically configures HTTPS.

---

## 🎉 Done!

Your subdomain **john.mas.codes** now runs using the *same Laravel codebase* with its own virtual host.

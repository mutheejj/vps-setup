# Laravel Subdomain Setup — `*.mas.codes`

> Guide for deploying a Laravel project on a subdomain of `mas.codes` using Nginx + Certbot on Ubuntu/Debian.

---

## 1. Create the DNS A Record

In your domain registrar or Cloudflare dashboard:

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | `api` | `YOUR_SERVER_IP` | Auto |

This creates `api.mas.codes` → your VPS. Swap `api` for whatever subdomain you need (e.g. `app`, `admin`, `crm`).

> ⏱ DNS can take up to 10–30 minutes to propagate.

---

## 2. Create the Nginx Config

```bash
sudo nano /etc/nginx/sites-available/api.mas.codes
```

Paste the following (Laravel-specific — root points to `/public`):

```nginx
server {
    listen 80;
    server_name api.mas.codes;

    root /var/www/api.mas.codes/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;   # adjust PHP version
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }

    error_log /var/log/nginx/api.mas.codes.error.log;
    access_log /var/log/nginx/api.mas.codes.access.log;
}
```

> Check PHP version: `php -v`  
> Adjust `php8.2-fpm.sock` → e.g. `php8.1-fpm.sock` or `php8.3-fpm.sock`

---

## 3. Enable the Site

```bash
sudo ln -s /etc/nginx/sites-available/api.mas.codes /etc/nginx/sites-enabled/

# Test config before restarting
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx
```

---

## 4. Deploy the Laravel Project

```bash
# Create the web root
sudo mkdir -p /var/www/api.mas.codes

# Clone your repo into it
cd /var/www/api.mas.codes
git clone https://github.com/mutheejj/your-repo.git .

# Install dependencies
composer install --optimize-autoloader --no-dev

# Set up environment
cp .env.example .env
php artisan key:generate
```

---

## 5. Set File Permissions

```bash
sudo chown -R www-data:www-data /var/www/api.mas.codes
sudo chmod -R 755 /var/www/api.mas.codes/storage
sudo chmod -R 755 /var/www/api.mas.codes/bootstrap/cache
```

---

## 6. Install SSL with Certbot

```bash
sudo certbot --nginx -d api.mas.codes
```

Certbot will auto-update your Nginx config to handle HTTPS on port 443 and redirect HTTP → HTTPS.

> Make sure port 80 and 443 are open in your firewall:
> ```bash
> sudo ufw allow 'Nginx Full'
> ```

---

## 7. Update Laravel `.env`

```env
APP_NAME="Your App"
APP_ENV=production
APP_URL=https://api.mas.codes

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_db
DB_USERNAME=your_user
DB_PASSWORD=your_password
```

Then clear and cache config:

```bash
php artisan config:cache
php artisan route:cache
php artisan storage:link   # if using file uploads
```

---

## 8. Run Migrations

```bash
php artisan migrate --force
```

---

## Quick Reference — Reuse for Other Subdomains

Just swap `api` with your target subdomain everywhere:

```bash
SUBDOMAIN="app"   # change this

sudo nano /etc/nginx/sites-available/$SUBDOMAIN.mas.codes
sudo ln -s /etc/nginx/sites-available/$SUBDOMAIN.mas.codes /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl restart nginx
sudo certbot --nginx -d $SUBDOMAIN.mas.codes
```

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| 502 Bad Gateway | PHP-FPM not running — `sudo systemctl start php8.2-fpm` |
| 403 Forbidden | Wrong permissions — re-run `chown` and `chmod` from step 5 |
| SSL fails | DNS not propagated yet — wait and retry |
| Laravel 500 error | Check logs: `tail -f /var/log/nginx/api.mas.codes.error.log` and `tail -f /var/www/api.mas.codes/storage/logs/laravel.log` |
| Nginx config error | Run `sudo nginx -t` to pinpoint the issue |

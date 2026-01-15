# Laravel Deployment permissions

Run these commands in order after pulling new code or deploying:

```bash
# 1. Enter project directory
cd /var/www/KICC

# 2. Set web server as owner of writable directories
sudo chown -R www-data:www-data storage bootstrap/cache

# 3. Set proper permissions (read/write for owner & group)
sudo chmod -R 775 storage bootstrap/cache

# 4. Create symlink for public storage
php artisan storage:link

# 5. Clear all caches to apply new config/routes
php artisan config:clear
php artisan cache:clear
# Optional: also clear these if needed
# php artisan route:clear
# php artisan view:clear

# 6. Verify storage symlink was created correctly
ls -l public/storage
# Should output something like:
# lrwxrwxrwx 1 www-data www-data 24 ... public/storage -> ../storage/app/public

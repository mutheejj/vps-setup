
sudo chown -R www-data:www-data /var/www/project
sudo find /var/www/project -type d -exec chmod 755 {} \;
sudo find /var/www/project -type f -exec chmod 644 {} \;
sudo chmod -R 775 /var/www/project/storage
sudo chmod -R 775 /var/www/project/bootstrap/cache

### Configurate Host

```
    nano /etc/hosts
```
add code like this

```
    [ip server] [domain]
```

### configurate Mysql
```
    mysql
    CREATE DATABASE wp_[name db];
    CREATE USER 'wp_[name user]'@'%' IDENTIFIED BY '[password]';
    GRANT ALL PRIVILEGES ON wp_[name db].* TO 'wp_[name user]'@'%' WITH GRANT OPTION;
```

### configurate Wordpress
make folder on /var/www/[name folder]
download wordpress, unzip, and move to your folder
```
    wget wordpress.org/latest.zip
    unzip latest.zip
    mv wordpress /var/www/[name folder]
```
change acces folder
```
    chown -R www-data:www-data /var/www/[name folder]
```
change file wp-config-sample.php to wp-config.php
```
    mv /var/www/[name folder]/wp-config-sample.php /var/www/[name folder]/wp-config.php
```
edit file wp-config.php
```
    nano /var/www/[name folder]/wp-config.php
```
get auth wordpress in this <a href="https://api.wordpress.org/secret-key/1.1/salt/"  target="_blank"> link </a>
edit file like this
```
    define( 'DB_NAME', 'wp_[name site]' );
    define( 'DB_USER', 'wp_[name site]' );
    define( 'DB_PASSWORD', '[password]' );
    [auth wordpress]
```

### configurate Apache
create file
```
    nano /etc/apache2/sites-available/[domain name].conf
```
copy this configurate
```
    <VirtualHost *:80>
        ServerAdmin webmaster@[domain name]
        ServerName [domain name]
        ServerAlias [domain name]
        DocumentRoot /var/www/[name folder]/

        <Directory /var/www/[name folder]/>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        <IfModule mod_dir.c>
            DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
        </IfModule>

    </VirtualHost>
```
Enable the site and reload Apache
```
  a2ensite [domain name].conf
  systemctl reload apache2
```
permission folder wordpress file manager and instalasion plugin self
```
sudo chown -R www-data:www-data /var/www/folder
sudo chmod 775 -R /var/www/folder
```
!!importen after install wordpress have to change permission
wp-config,wp-load,.htaccess 400 (for edit, change permission to 600)
```
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;
chown <username>:<username>  -R *
chown www-data:www-data wp-content
```
Plus ON Security httaccess
```
sudo chown <username>:<username> /var/www/[path]/.htaccess
sudo chmod 644 /var/www/[path]/.htaccess
sudo chmod -w /var/www/[path]/.htaccess
sudo chattr +i /var/www/[path]/.htaccess
```
Plus Off Security httaccess
```
chattr -i /var/www/[path]/.htaccess
chmod u+w /var/www/[path]/.htaccess
```
Update PHP
```
a2dismod php[old version]
a2enmod php[new version]
systemctl reload apache2
```
### PHP extensions requirements cyber seo
```
sudo apt install php php-curl php-gd php-imagick php-json php-xml php-mbstring php-zip php-mysql
ln -sf /etc/php/version/cli/php.ini /etc/php/version/apache2/php.ini
```
## PHP Incress Performce /etc/php/version/cli/php.ini
```
max_execution_time = 86400 ; 24 Hour
max_input_time = 86400 ; 24 hour
memory_limit = 512M
upload_max_filesize = 500M
post_max_size = 500M
realpath_cache_size = 4096k
realpath_cache_ttl = 600
```
### SSL https
```
        sudo apt install certbot python3-certbot-apache
        sudo certbot --apache
```

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
    CREATE DATABASE wp_[name site];
    CREATE USER 'wp_[name site]'@'%' IDENTIFIED BY '[password]';
    GRANT ALL PRIVILEGES ON wp_[name site].* TO 'wp_[name site]'@'%' WITH GRANT OPTION;
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
    mv /var/www/[name folder]/wordpress/wp-config-sample.php /var/www/[name folder]/wordpress/wp-config.php
```
edit file wp-config.php
```
    nano /var/www/[name folder]/wordpress/wp-config.php
```
get auth wordpress in this <a href="https://api.wordpress.org/secret-key/1.1/salt/"> link </a>
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
        DocumentRoot /var/www/[name folder]/wordpress

        <Directory /var/www/[name folder]/wordpress/>
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
